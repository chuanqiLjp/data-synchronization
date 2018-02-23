###版权声明，转载请著名出处：http://www.jianshu.com/p/73a803b0b026
#引言
俗话说：代码是程序员的最好的教程，这篇文章记录的是我在学习使用Retrofit的代码笔记，其中里面的很多注解或原理我也没有弄明白，但是这不影响我的正常使用啊，当然这篇文章针对的是初学者，如果是老司机的话就请绕道了，如果大家有更好的看法或建议可以在文末进行评论，我会及时更新到文章中，近期我也会更新RxJava的简单使用和RxJava结合Retrofit使用的教程，希望大家喜欢，欢迎大家持续关注！
#开始使用
导入依赖
```
    //  retrofit的依赖
    compile 'com.squareup.retrofit2:retrofit:2.1.0'
    //将JSON字符串转换为对象需要使用的依赖
    compile 'com.squareup.retrofit2:converter-gson:2.1.0'
```
IpMode类由Json字符串GsonFromat生成
```
{
    "code": 0,
    "data": {
        "country": "中国",
        "country_id": "CN",
        "area": "西南",
        "area_id": "500000",
        "region": "四川省",
        "region_id": "510000",
        "city": "成都市",
        "city_id": "510100",
        "county": "",
        "county_id": "-1",
        "isp": "电信",
        "isp_id": "100017",
        "ip": "118.114.168.175"
    }
}
```
#1. Get简单请求
**注：Retrofit2 的baseUlr 必须以 /（斜线） 结束，不然会抛出一个IllegalArgumentException,所以如果你看到别的教程没有以 / 结束，那么多半是直接从Retrofit 1.X 照搬过来的。**
```
//请求的接口
public interface IpService {
    @Headers({
            "Accept-Encoding: application/json",
            "User-Agent: MoonRetrofit"  //必须要有这个Hearder，否则报错 java.lang.IllegalArgumentException: @Headers annotation is empty.
    })
    @GET("getIpInfo.php?ip=59.108.54.37")
    Call<IpMode> getIpMsgGet();
    /**
     * 采用指定的全路径访问
     * @param url   网址的全路径
     * @return
     */
    @Headers({
            "User-Agent: MoonRetrofit"
    })
    @GET
    Call<IpMode> getIpMsgGet1(@Url String url);
｝
```
```
 public void getSimple(View view) {
        // Get简单请求
        final String url = "http://ip.taobao.com/service/";
        Retrofit retrofit = new Retrofit
                .Builder()
                .baseUrl(url)
                .addConverterFactory(GsonConverterFactory.create())
                .build();
        IpService ipService = retrofit.create(IpService.class);
        Call<IpMode> ipMsg = ipService.getIpMsgGet();
        ipMsg.enqueue(new Callback<IpMode>() {
            @Override
            public void onResponse(Call<IpMode> call, Response<IpMode> response) {
                IpMode ipMode = response.body();
                Log.e(TAG, "onResponse: " + ipMode);
                Toast.makeText(MainActivity.this, ipMode + "", Toast.LENGTH_LONG).show();
            }
            @Override
            public void onFailure(Call<IpMode> call, Throwable t) {
                Log.e(TAG, "onFailure: ");
                t.printStackTrace();
                Toast.makeText(MainActivity.this, t + "", Toast.LENGTH_LONG).show();
            }
        });
        getSimple();// Get简单请求 ，采用全路径来访问
    }
    private void getSimple() {
        // Get简单请求 ，采用全路径来访问
        final String url = "http://ip.taobao.com/service/";
        Retrofit retrofit = new Retrofit
                .Builder()
                .baseUrl(url)
                .addConverterFactory(GsonConverterFactory.create())
                .build();
        IpService ipService = retrofit.create(IpService.class);
        Call<IpMode> ipMsg = ipService.getIpMsgGet1("http://ip.taobao.com/service/getIpInfo.php?ip=118.114.168.175");
        ipMsg.enqueue(new Callback<IpMode>() {
            @Override
            public void onResponse(Call<IpMode> call, Response<IpMode> response) {
                IpMode ipMode = response.body();
                Log.e(TAG, "onResponse: " + ipMode);
                Toast.makeText(MainActivity.this, ipMode + "", Toast.LENGTH_LONG).show();
            }
            @Override
            public void onFailure(Call<IpMode> call, Throwable t) {
                Log.e(TAG, "onFailure: ");
                t.printStackTrace();
                Toast.makeText(MainActivity.this, t + "", Toast.LENGTH_LONG).show();
            }
        });
    }
```
#2. Get带参数请求(路径)可以通过参数动态修改网址的路径
```
    /**
     * 指定路径的访问
     *
     * @param pathStr 路径
     * @return
     */
    @Headers({
            "Accept-Encoding: application/json",
            "User-Agent: MoonRetrofit"
    })
    @GET("{path}/getIpInfo.php?ip=59.108.54.37")//{path}与@Path("path")参数对应，接受参数后{path}被替换
    Call<IpMode> getIpMsgGet(@Path("path") String pathStr);
```
```
    public void getParamesPath(View view) {
//        Get带参数请求(路径)
        final String baseUrl = "http://ip.taobao.com/";
        Retrofit retrofit = new Retrofit
                .Builder()
                .baseUrl(baseUrl)
                .addConverterFactory(GsonConverterFactory.create())
                .build();
        IpService ipService = retrofit.create(IpService.class);
        Call<IpMode> ipMsg = ipService.getIpMsgGet("service");   //,"getIpInfo.php?ip=59.108.54.37"
        ipMsg.enqueue(new Callback<IpMode>() {
            @Override
            public void onResponse(Call<IpMode> call, Response<IpMode> response) {
                IpMode ipMode = response.body();
                Log.e(TAG, "onResponse: " + ipMode);
                Toast.makeText(MainActivity.this, ipMode + "", Toast.LENGTH_LONG).show();
            }
            @Override
            public void onFailure(Call<IpMode> call, Throwable t) {
                Log.e(TAG, "onFailure: ");
                t.printStackTrace();
                Toast.makeText(MainActivity.this, t + "", Toast.LENGTH_LONG).show();
            }
        });
    }
```
#3. Get带参数请求(IP)可以动态修改网址？后面的参数
```
    /**
     * 指定IP访问
     * 会组成全路径：http://ip.taobao.com/service/getIpInfo.php?ip=118.114.168.175
     * 在路径后自动添加  ?ip=ipStr
     *
     * @param ipStr
     * @return
     */
    @Headers({
            "Accept-Encoding: application/json",
            "User-Agent: MoonRetrofit"
    })
    @GET("service/getIpInfo.php")
    Call<IpMode> getIpMsgFromIpGet(@Query("ip") String ipStr);
```
```
    public void getParamesIp(View view) {
//        Get带参数请求(IP)
        final String baseUrl = "http://ip.taobao.com/";
        Retrofit retrofit = new Retrofit
                .Builder()
                .baseUrl(baseUrl)
                .addConverterFactory(GsonConverterFactory.create())
                .build();
        IpService ipService = retrofit.create(IpService.class);
        Call<IpMode> ipMsg = ipService.getIpMsgFromIpGet("59.108.54.37");   //,"getIpInfo.php?ip=59.108.54.37"
        ipMsg.enqueue(new Callback<IpMode>() {
            @Override
            public void onResponse(Call<IpMode> call, Response<IpMode> response) {
                IpMode ipMode = response.body();
                Log.e(TAG, "onResponse: " + ipMode);
                Toast.makeText(MainActivity.this, ipMode + "", Toast.LENGTH_LONG).show();
            }
            @Override
            public void onFailure(Call<IpMode> call, Throwable t) {
                Log.e(TAG, "onFailure: ");
                t.printStackTrace();
                Toast.makeText(MainActivity.this, t + "", Toast.LENGTH_LONG).show();
            }
        });
    }
```
#4. Get带参数请求(路径和IP)可以通过参数动态修改网址的路径和网址？后面的请求参数
```
    @Headers({
            "Accept-Encoding: application/json",
            "User-Agent: MoonRetrofit"
    })
    @GET("{path}")
    Call<IpMode> getIpMsgGet(@Path("path") String pathStr, @Query("ip") String ipStr);
```
```
    public void getParamesPathIp(View view) {
//        Get带参数请求(路径和IP)
        final String baseUrl = "http://ip.taobao.com/";
        Retrofit retrofit = new Retrofit
                .Builder()
                .baseUrl(baseUrl)
                .addConverterFactory(GsonConverterFactory.create())
                .build();
        IpService ipService = retrofit.create(IpService.class);
        Call<IpMode> ipMsg = ipService.getIpMsgGet("service/getIpInfo.php", "59.108.54.37");   //,"getIpInfo.php?ip=59.108.54.37"
        ipMsg.enqueue(new Callback<IpMode>() {
            @Override
            public void onResponse(Call<IpMode> call, Response<IpMode> response) {
                IpMode ipMode = response.body();
                Log.e(TAG, "onResponse: " + ipMode);
                Toast.makeText(MainActivity.this, ipMode + "", Toast.LENGTH_LONG).show();
            }
            @Override
            public void onFailure(Call<IpMode> call, Throwable t) {
                Log.e(TAG, "onFailure: ");
                t.printStackTrace();
                Toast.makeText(MainActivity.this, t + "", Toast.LENGTH_LONG).show();
            }
        });
    }
```
#5.Get带参数请求(动态指定查询条件组)
```
    /**
     * 动态指定查询条件组
     *
     * @param quaryMap
     * @return
     */
    @Headers({
            "Accept-Encoding: application/json",
            "User-Agent: MoonRetrofit"
    })
    @GET("service/getIpInfo.php")
    Call<IpMode> getIpMsgGet(@QueryMap Map<String, String> quaryMap);
```
```
    public void getParamesQuaryMap(View view) {
//        Get带参数请求(动态指定查询条件组)
        final String baseUrl = "http://ip.taobao.com/";
        Retrofit retrofit = new Retrofit
                .Builder()
                .baseUrl(baseUrl)
                .addConverterFactory(GsonConverterFactory.create())
                .build();
        IpService ipService = retrofit.create(IpService.class);
        HashMap<String, String> hashMap = new HashMap<>();
        hashMap.put("ip", "59.108.54.37"); //Key 和 Value要对应，其中Key名要与网站对应
        Call<IpMode> ipMsg = ipService.getIpMsgGet(hashMap);
        ipMsg.enqueue(new Callback<IpMode>() {
            @Override
            public void onResponse(Call<IpMode> call, Response<IpMode> response) {
                IpMode ipMode = response.body();
                Log.e(TAG, "onResponse: " + ipMode);
                Toast.makeText(MainActivity.this, ipMode + "", Toast.LENGTH_LONG).show();
            }
            @Override
            public void onFailure(Call<IpMode> call, Throwable t) {
                Log.e(TAG, "onFailure: ");
                t.printStackTrace();
                Toast.makeText(MainActivity.this, t + "", Toast.LENGTH_LONG).show();
            }
        });
    }
```
#6.Post带参数请求
```
    /**
     * 带参数的POST请求
     * 用@Field("ip")标注请求的参数键值
     *
     * @param ip
     * @return
     */
    @Headers({
            "Accept-Encoding: application/json", //
            "User-Agent: MoonRetrofit"
    })
    @FormUrlEncoded  //注明是一个表单请求
    @POST("service/getIpInfo.php")
    Call<IpMode> getIpMsgPost(@Field("ip") String ip);
    @Headers({
            "Accept-Encoding: application/json", //
            "User-Agent: MoonRetrofit"
    })
    @FormUrlEncoded  //注明是一个表单请求
    @POST("service/getIpInfo.php")
    Call<IpMode> getIpMsgPost(@FieldMap Map<String, String> quaryMap);
```
```
    public void postParames(View view) {
//        Post带参数请求
        String url = "http://ip.taobao.com/";
        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl(url)
                .addConverterFactory(GsonConverterFactory.create())
                .build();
        IpService ipService = retrofit.create(IpService.class);
//        Call<IpMode> ipMsg = ipService.getIpMsgPost("59.108.54.37");
        HashMap<String, String> hashMap = new HashMap<>();
        hashMap.put("ip", "59.108.54.37");
        Call<IpMode> ipMsg = ipService.getIpMsgPost(hashMap);
        ipMsg.enqueue(new Callback<IpMode>() {
            @Override
            public void onResponse(Call<IpMode> call, Response<IpMode> response) {
                IpMode ipMode = response.body();
                Log.e(TAG, "onResponse: " + ipMode);
                Toast.makeText(MainActivity.this, ipMode + "", Toast.LENGTH_LONG).show();
            }
            @Override
            public void onFailure(Call<IpMode> call, Throwable t) {
                Log.e(TAG, "onFailure: ");
                t.printStackTrace();
                Toast.makeText(MainActivity.this, t + "", Toast.LENGTH_LONG).show();
            }
        });
    }
```
#7. 下载图片或文件
```
    /**
     * 下载图片，返回的是OkHttp库的Call<ResponseBody>， ResponseBody里面是字节数据
     * 百度的首页图
     *
     * @return
     */
    @Headers({
            "Accept-Encoding: application/json",
            "User-Agent: MoonRetrofit"
    })
    @Streaming
    @GET("5aV1bjqh_Q23odCf/static/superman/img/logo/logo_white_fe6da1ec.png")
    Call<ResponseBody> getImgMsgGet();
    /**
     * 使用全路径的网址下载图片
     *
     * @param fileUrl
     * @return
     */
    @GET
    Call<ResponseBody> getImgMsgGet(@Url String fileUrl);
```
```
    public void getDownloadImg(View view) {
//        Get下载图片
        String url = "https://ss0.bdstatic.com/";
        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl(url)
                .build();
        IpService ipService = retrofit.create(IpService.class);
        Call<ResponseBody> imgMsgGet = ipService.getImgMsgGet();
        imgMsgGet.enqueue(new Callback<ResponseBody>() {
            @Override
            public void onResponse(Call<ResponseBody> call, Response<ResponseBody> response) {
                try {
                    byte[] bytes = response.body().bytes();
                    Bitmap bitmap = BitmapFactory.decodeByteArray(bytes, 0, bytes.length);
                    imageview.setImageBitmap(bitmap);
                } catch (IOException e) {
                    e.printStackTrace();
                }
                Log.e(TAG, "onResponse: 图片请求成功");
                Toast.makeText(MainActivity.this, "图片请求成功", Toast.LENGTH_LONG).show();
            }
            @Override
            public void onFailure(Call<ResponseBody> call, Throwable t) {
                Log.e(TAG, "onFailure: ");
                t.printStackTrace();
                Toast.makeText(MainActivity.this, t + "", Toast.LENGTH_LONG).show();
            }
        });
        //使用全路径
        Call<ResponseBody> imgMsgGet2 = ipService.getImgMsgGet("http://img1.3lian.com/2015/a1/105/d/40.jpg");
        imgMsgGet2.enqueue(new Callback<ResponseBody>() {
            @Override
            public void onResponse(Call<ResponseBody> call, Response<ResponseBody> response) {
                File file = new File(MainActivity.this.getCacheDir(), "laowang.jpg");
                try {
                    byte[] bytes = response.body().bytes();
                    Bitmap bitmap = BitmapFactory.decodeByteArray(bytes, 0, bytes.length);
                    imageview.setImageBitmap(bitmap);
                    //保存为文件
                    FileOutputStream fileOutputStream = new FileOutputStream(file, false);
                    fileOutputStream.write(bytes);
                    fileOutputStream.flush();
                    fileOutputStream.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
                Log.e(TAG, "onResponse: 图片请求成功２,ex=" + file.exists() + ",path=" + file.getAbsolutePath());
                Toast.makeText(MainActivity.this, "图片请求成功２", Toast.LENGTH_LONG).show();
            }
            @Override
            public void onFailure(Call<ResponseBody> call, Throwable t) {
                Log.e(TAG, "onFailure: ２");
                t.printStackTrace();
                Toast.makeText(MainActivity.this, t + "２", Toast.LENGTH_LONG).show();
            }
        });
    }
```
#8. POST上传文件
```
    /**
     *  POST上传文件
     *  注解@Multipart 表示允许多个@Part
     *  如需要多个文件上传可以使用@PartMap
     * @param file
     * @return
     */
    @Multipart
    @POST("fileService")
    Call<IpMode> uploadFile(@Part MultipartBody.Part file);
```
```
    public void postUpImg(View view) {// POST上传图片
        File file = new File(MainActivity.this.getCacheDir(), "laowang.jpg");
        RequestBody body = RequestBody.create(MediaType.parse("application/otcet-stream"), file);//构建文件的RequestBody
        MultipartBody.Part part = MultipartBody.Part.createFormData("file", file.getName(), body);//构建文件的Part
        String url = "https://ss0.bdstatic.com/";
        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl(url)
                .addConverterFactory(GsonConverterFactory.create())
                .build();
        IpService ipService = retrofit.create(IpService.class);
        Call<IpMode> call = ipService.uploadFile(part);
        call.enqueue(new Callback<IpMode>() {
            @Override
            public void onResponse(Call<IpMode> call, Response<IpMode> response) {
                Log.e(TAG, "onResponse: 图片上传成功," + response.headers());
                Toast.makeText(MainActivity.this, "图片上传成功", Toast.LENGTH_LONG).show();
            }
            @Override
            public void onFailure(Call<IpMode> call, Throwable t) {
                Log.e(TAG, "onFailure: ");
                t.printStackTrace();
                Toast.makeText(MainActivity.this, t + "", Toast.LENGTH_LONG).show();
            }
        });
    }
```
我的CSDN博客地址：http://blog.csdn.net/wo_ha/article/details/77866493