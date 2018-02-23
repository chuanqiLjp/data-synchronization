序言：本篇的笔记篇，用于记录我目前使用的异常处理器，以便后期查找使用;
### 在Android开发中，我们的程序偶尔会出现“很抱歉，XXXX已停止运行”，这是因为我们虽然在开发中尽力避免了RuntimeException，但还有有未捕获到的异常，这时就需要一个全局的异常捕获器，虽不能避免，但至少对用户友好些，而且此时可以收集到Exception信息，便于我们定位和解决问题。建议直接收藏此文，在实际开发中直接使用就可以了

```
import android.content.Context;
import android.content.Intent;
import android.content.pm.PackageInfo;
import android.content.pm.PackageManager;
import android.content.pm.PackageManager.NameNotFoundException;
import android.os.Build;
import android.os.Environment;
import android.os.Looper;
import android.os.StatFs;
import android.os.SystemClock;
import android.util.Log;
import android.widget.Toast;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.PrintWriter;
import java.io.StringWriter;
import java.io.Writer;
import java.lang.Thread.UncaughtExceptionHandler;
import java.lang.reflect.Field;
import java.util.HashMap;
import java.util.Map;

/**
 * 记录程序崩溃的日志 UncaughtException处理类,当程序发生Uncaught异常的时候,由该类来接管程序,并记录发送错误报告.
 *
 * @author ljp
 */
public class CrashHandler implements UncaughtExceptionHandler {

    private static final String TAG = "CrashHandler";
    private Thread.UncaughtExceptionHandler mDefaultHandler;// 系统默认的UncaughtException处理类
    private volatile static CrashHandler INSTANCE;// CrashHandler实例
    private Context mContext;// 程序的Context对象
    private Map<String, String> info = new HashMap<String, String>();// 用来存储设备信息和异常信息
    private File file;
    private static final long FILESIZE = 1024 * 1024 * 30;// 文件的大小为30M；
    private static final String FILENAME = "CrashLog.txt";

    /**
     * 保证只有一个CrashHandler实例
     */
    private CrashHandler(Context context) {
        mContext = context.getApplicationContext();
        file = getSaveFile();
        if (file.exists() && file.isFile() && file.length() > FILESIZE) {
            file.delete();
        }
    }

    /**
     * 获取保存日志文件的路径
     *
     * @return
     */
    private File getSaveFile() {
        File file = null;
        if (Environment.getExternalStorageState().equals(Environment.MEDIA_MOUNTED)) {//SD卡可用
            File path = new File(ConsValue.FilePath.logcatPath);
            if (getUsableSpace(path) >= FILESIZE) {
                file = new File(path, FILENAME);
            } else {
                file = new File(mContext.getCacheDir(), FILENAME);
            }
        } else {
            file = new File(mContext.getCacheDir(), FILENAME);
        }
        return file;
    }

    /**
     * 获取指定路径下的可用空间大小
     *
     * @param path
     * @return
     */
    private long getUsableSpace(File path) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.GINGERBREAD) {
            return path.getUsableSpace();
        }
        StatFs statFs = new StatFs(path.getPath());
        return statFs.getBlockSizeLong() * statFs.getAvailableBlocksLong();
    }

    /**
     * 获取CrashHandler实例 ,单例模式
     */
    public static CrashHandler getInstance(Context context) {
        if (INSTANCE == null) {
            synchronized (CrashHandler.class) {
                if (INSTANCE == null) {
                    if (context == null) {
                        throw new NullPointerException("context cannet NULL");
                    }
                    INSTANCE = new CrashHandler(context);
                }
            }
        }
        return INSTANCE;
    }

    /**
     * 初始化
     */
    public void init() {
        mDefaultHandler = Thread.getDefaultUncaughtExceptionHandler();// 获取系统默认的UncaughtException处理器
        Thread.setDefaultUncaughtExceptionHandler(this);// 设置该CrashHandler为程序的默认处理器
    }

    /**
     * 当UncaughtException发生时会转入该重写的方法来处理
     */
    @Override
    public void uncaughtException(Thread thread, Throwable ex) {
        if (!handleException(ex) && mDefaultHandler != null) {
            mDefaultHandler.uncaughtException(thread, ex); // 如果自定义的没有处理则让系统默认的异常处理器来处理
        } else {
            SystemClock.sleep(3000);// 如果处理了，让程序继续运行3秒再退出，保证文件保存并上传到服务器
            Utils.exitProgress();// 退出程序
        }
    }

    /**
     * 自定义错误处理,收集错误信息 发送错误报告等操作均在此完成.
     *
     * @param ex 异常信息
     * @return true 如果处理了该异常信息;否则返回false.
     */
    public boolean handleException(Throwable ex) {
        if (ex == null) {
            return false;
        }
        new Thread() {
            public void run() {
                Looper.prepare();
                Toast.makeText(mContext, "很抱歉,程序出现异常,即将退出", Toast.LENGTH_SHORT).show();
                Looper.loop();
            }
        }.start();
        collectDeviceInfo(mContext); // 收集设备参数信息
        saveCrashInfo2File(ex);// 保存日志文件
        uploadExceptionToServer();//上传数据到服务器
        return true;
    }


    private void uploadExceptionToServer() {
        //TODO 发送数据到服务器
    }

    /**
     * 设备的详细版本号，如：1.0.0，对应manifest或moude的build.gradle中的 versionName
     */
    private final String KEY_versionName = "versionName";
    /**
     * 设备大的版本号，如：1，对应manifest或moude的build.gradle中的 versionCode
     */
    private final String KEY_versionCode = "versionCode";
    /**
     * 系统的版本号，如：4.4.4
     */
    private final String KEY_osVersion = "osVersion";
    /**
     * 系统对应的ＳＤＫ编码，如：19
     */
    private final String KEY_osSDKVersion = "osSDKVersion";
    /**
     * 手机制造厂商，如：HUAWEI
     */
    private final String KEY_phoneMaker = "phoneMaker";
    /**
     * 手机品牌，如：Honor
     */
    private final String KEY_BRAND = "brand";
    /**
     * 手机型号，如：Che1-CL10
     */
    private final String KEY_phoneModel = "phoneModel";
    /**
     * CPU架构，如：armeabi-v7a
     */
    private final String KEY_cupAbi = "cupAbi";

    /**
     * 收集设备参数信息
     *
     * @param context
     */
    public void collectDeviceInfo(Context context) {
        try {
            PackageManager pm = context.getPackageManager();// 获得包管理器
            PackageInfo pi = pm.getPackageInfo(context.getPackageName(),
                    PackageManager.GET_ACTIVITIES);// 得到该应用的信息，即主Activity
            if (pi != null) {
                String versionName = pi.versionName == null ? "null"
                        : pi.versionName;
                String versionCode = pi.versionCode + "";
                info.put(KEY_versionName, versionName);
                info.put(KEY_versionCode, versionCode);
            }
            info.put(KEY_osVersion, Build.VERSION.RELEASE);
            info.put(KEY_osSDKVersion, "" + Build.VERSION.SDK_INT);
            info.put(KEY_phoneMaker, Build.MANUFACTURER);
            info.put(KEY_BRAND, Build.BRAND);
            info.put(KEY_phoneModel, Build.MODEL);
            info.put(KEY_cupAbi, Build.CPU_ABI);
//            Build.DISPLAY;//版本号，如：Che1-CL10V100R001CHNC92B285
        } catch (NameNotFoundException e) {
            e.printStackTrace();
        }
        //遍历所有 Build中的字段
        Field[] fields = Build.class.getDeclaredFields();// 反射机制
        for (Field field : fields) {
            try {
                field.setAccessible(true);
                info.put(field.getName(), field.get("").toString());
                Log.d(TAG, field.getName() + ":" + field.get(""));
            } catch (IllegalArgumentException e) {
                e.printStackTrace();
            } catch (IllegalAccessException e) {
                e.printStackTrace();
            }
        }

    }

    private void saveCrashInfo2File(Throwable ex) {
        StringBuffer sb = new StringBuffer();
        sb.append(DateUtil.getDateEN() + "：发生崩溃的异常，设备的信息如下：******************************************************分割线***********************" + "\r\n");
        for (Map.Entry<String, String> entry : info.entrySet()) {
            String key = entry.getKey();
            String value = entry.getValue();
            sb.append(key + "\t=\t" + value + "\r\n");
        }
        Writer writer = new StringWriter();
        PrintWriter pw = new PrintWriter(writer);
        ex.printStackTrace(pw);
        Throwable cause = ex.getCause();
        // 循环着把所有的异常信息写入writer中
        while (cause != null) {
            cause.printStackTrace(pw);
            cause = cause.getCause();
        }
        pw.close();// 记得关闭
        String result = writer.toString();
        sb.append("发生崩溃的异常信息如下：" + "\r\n");
        sb.append(result);
        Log.e(TAG, result);
        // 保存文件
        try {
            //判断文件夹是否存在
            if (!file.getParentFile().exists()) {
                file.getParentFile().mkdir();
            }
            FileOutputStream fos = new FileOutputStream(file, true);
            fos.write(sb.toString().getBytes("UTF-8"));
            fos.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```
使用时，一般在Application中初始化调用就可以了
```
 CrashHandler.getInstance(mContext).init();
```
##强烈建议收藏使用