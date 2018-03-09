---
title: POI读取Excel表格和导出Excel文件
layout: post
date: 2018-01-25 14:31:58
comments: true
categories:
  - Android
  - 文档操作
  - Excel
tags: [读取Excel表格,导出Excel文件]
keywords: 读取Excel表格,导出Excel文件
description:
---

>我的简书：https://www.jianshu.com/u/c91e642c4d90
我的CSDN：http://blog.csdn.net/wo_ha
我的GitHub：https://github.com/chuanqiLjp
我的个人博客：https://chuanqiljp.github.io/

# 版权声明：转载需要在醒目注明出处


###序言：
- 这两天在弄一个股票数据分析，而Choice中导出的数据是Excel的表格，数据多达上百个，如果要手动的去操作的话，操作量非常大，所以就想着怎么用代码去实现，主要需求是：读取100多个相同格式的excel文件然后进行合并。翻译下就是Excel文件的读写。因此本篇就讲Excel读取和Excel的导出，一些常见的格式设置，增加单元格的边框，设置单元格的字体等，都是非常简单的使用，更详细的请参阅[官方文档](http://poi.apache.org/spreadsheet/quick-guide.html#NewWorkbook)
# 1、下载需要的Jar包，[点击立即下载](http://download.csdn.net/download/wo_ha/10222029)
> 里面包含了五个POI操作的jar包，用于操作excel，支持2003、2007版本，有poi-3.14、poi-ooxml-3.14、poi-ooxml-schemas-3.14、poi-scratchpad-3.8、xmlbeans-2.6.0，由于CSDN最低分的限制，所以需要少许积分，没有积分的同学可以在文末留下你的邮箱，有空我会发给你

#2、导出Excel文件，代码中有大量注释，就不一一讲解了，读写流程：Workbook(工作簿)——>Sheet(工作表)——>Row(行)——>Cell(单元格)

- 2003-2007 版本的 工作簿，文件的后缀名为xls的文件可以单独使用 HSSFWorkbook workbook = new HSSFWorkbook()
-  2007 以上版本的 工作簿，文件的后缀名为Xlsx的文件可以单独使用  XSSFWorkbook workbook=new XSSFWorkbook();
- 下面对应的类加上对应的前缀HSSF或XSSF，而没有前缀的可以通用xls和xlsx的文件， 需要加前缀的类如：HSSFWorkbook、HSSFSheet、HSSFRow、HSSFCell、HSSFFont、HSSFCellStyle、HSSFDataFormat、

```
	/**
	 * 导出Excel
	 * @param sheetName	表名
	 * @param sheetHeads	表头
	 * @param listContent	数据集合
	 * @param os	输出流，可输出到文件、网络
	 */
	public static void writeExcel(String sheetName, String[] sheetHeads,
			List<Student> listContent, OutputStream os) {
		try {
			/**
			 * 2003-2007 版本的 工作簿，文件的后缀名为xls的文件可以单独使用 HSSFWorkbook workbook = new HSSFWorkbook()
			 * 2007 以上版本的 工作簿，文件的后缀名为Xlsx的文件可以单独使用  XSSFWorkbook workbook=new XSSFWorkbook();
			 * 下面对应的类加上对应的前缀HSSF或XSSF，而没有前缀的可以通用xls和xlsx的文件， 需要加前缀的类如：HSSFWorkbook、HSSFSheet、HSSFRow、HSSFCell、HSSFFont、HSSFCellStyle、HSSFDataFormat、
			 * 读写流程：Workbook(工作簿)——>Sheet(工作表)——>Row(行)——>Cell(单元格)
			 */
			Workbook workbook= new HSSFWorkbook();// 创建一个2007以上版本的工作簿，创建需要指定具体的版本，读取可以不用
			Sheet sheet = workbook.createSheet(sheetName);// 创建一个工作表

			sheet.setDefaultRowHeight((short) (1 * 256));// 设置默认行高，表示1个字符的高度

			Font font = workbook.createFont();// 创建一个字体对象，
			font.setBold(true);// 是否加粗
			font.setFontName("宋体");// 设置字体
			font.setFontHeightInPoints((short) 16);// 设置字体大小

			CellStyle cellStyle = workbook.createCellStyle();// 创建一个单元格的样式
			cellStyle.setAlignment(CellStyle.ALIGN_CENTER);// 居中对齐
			cellStyle.setWrapText(true);// 设置自动换行
			cellStyle.setFont(font);// 将字体设置到样式中
			cellStyle.setFillForegroundColor(HSSFColor.RED.index);// 设置背景色

			int rowCount=0;//用于计数已有多少行的数据，其真实有的数据为rowCount条，访问时下标需要减1

			// 填充表头数据
			Cell cell = null;
			String head;
			Row headRow = sheet.createRow(rowCount++);// 创建第一行，下标从0 开始
			for (int i = 0; i < sheetHeads.length; i++) {
				head = sheetHeads[i];
				sheet.setColumnWidth(i, (head.length() + 20) * 256);// 设置第i列的列宽，第一个参数代表列下标(从0开始),第2个参数代表宽度值需要乘256表示多少个字符宽度
				cell = headRow.createCell(i);// 创建一个单元格
				cell.setCellValue(head);// 为单元格设置一个值
				cell.setCellStyle(cellStyle);// 设置单元格的样式
			}

			// 填充内容数据
			cellStyle.setFillForegroundColor(HSSFColor.WHITE.index);// 设置背景色
			Student student = null;
			Row row = null;
			// 为日期单独设置单元格的格式
			DataFormat dataFormat = workbook.createDataFormat();
			CellStyle cellStyleData = workbook.createCellStyle();
			cellStyleData.setDataFormat(dataFormat.getFormat("yyyy年MM月dd日 hh:mm"));

			for (int i = 0; i < listContent.size(); i++) {
				student = listContent.get(i);
				row = sheet.createRow(rowCount++);// 创建第i+1行d
				cell = row.createCell(0);// 创建第i+1行是第1个单元格
				cell.setCellStyle(cellStyle);
				cell.setCellValue(student.getId());

				cell = row.createCell(1);// 创建第i+1行是第2个单元格
				cell.setCellStyle(cellStyle);
				cell.setCellValue(student.getName());

				cell = row.createCell(2);// 创建第i+1行是第3个单元格
				cell.setCellStyle(cellStyle);
				cell.setCellValue(student.isSex());

				cell = row.createCell(3);// 创建第i+1行是第4个单元格
				cell.setCellStyle(cellStyle);
				cell.setCellValue(student.getAge());

				cell = row.createCell(4);// 创建第i+1行是第5个单元格
				cell.setCellStyle(cellStyle);
				cell.setCellValue(student.getScore());

				cell = row.createCell(5);// 创建第i+1行是第6个单元格
				/**
				 * 读取出来就是一个字符串
				 * 读取代码：formatSS.parse(cell.getStringCellValue())
				 */
				//cell.setCellValue(formatSS.format(student.getDate()));
				/**
				 *  读取出来的是一个double类型的数据,读取代码：
				 *  if (DateUtil.isCellDateFormatted(cell)||cell.getCellStyle().getDataFormat() == 164) {
				 *  System.out.println(formatSS.format(cell.getDateCellValue()));}
				 *
				 */
				cell.setCellValue(student.getDate());
				cell.setCellStyle(cellStyleData);
			}
			//为单个的单元格增加边框
			row = sheet.createRow(rowCount++);
			cell=row.createCell(0);
			cell.setCellValue("为该单元格加上边框");
			CellStyle borderCellStyle = workbook.createCellStyle();
			borderCellStyle.setAlignment(CellStyle.ALIGN_CENTER);
			borderCellStyle.setBorderBottom(CellStyle.BORDER_THIN);
			borderCellStyle.setBorderTop(CellStyle.BORDER_THIN);
			borderCellStyle.setBorderLeft(CellStyle.BORDER_THIN);
			borderCellStyle.setBorderRight(CellStyle.BORDER_THIN);
			cell.setCellStyle(borderCellStyle);


			//增加最后一行数据
			row = sheet.createRow(rowCount++);
			row.createCell(0).setCellValue("@版权归LJP所有，该文件仅供学习参考使用");
	        // 合并单元格
	        CellRangeAddress rangeAddress =new CellRangeAddress(rowCount-1, rowCount-1, 0, 3); // 起始行, 终止行, 起始列, 终止列
	        sheet.addMergedRegion(rangeAddress);
	        // 使用RegionUtil类为合并后的单元格添加边框
	        RegionUtil.setBorderBottom(1, rangeAddress, sheet, workbook); // 下边框
	        RegionUtil.setBorderLeft(1, rangeAddress, sheet, workbook); // 左边框
	        RegionUtil.setBorderRight(1, rangeAddress, sheet, workbook); // 有边框
	        RegionUtil.setBorderTop(1, rangeAddress, sheet, workbook); // 上边框


			workbook.write(os);
			os.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
```

![写入的Excel文件](http://upload-images.jianshu.io/upload_images/4143664-79a2fb8ddc804170.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##3、读取Excel，通常来说是需要区分excel版本的，如2003-2007的版本文件后缀为xls，需要使用HSSFWorkbook，而2007+的版本文件后缀为xlsx，需要使用XSSFWorkbook，但是读取可以使用WorkbookFactory.create(file)

```
	public static void readExcel2(File excelFile) {
		try {
			Workbook workbook=WorkbookFactory.create(excelFile);
			Sheet sheet = workbook.getSheetAt(0);//或者根据表名称读取：sheet=workbook.getSheet("表1");

			/**
			 * 遍历数据时可以使用迭代器Iterator,forEach,也可以使用for循环，
			 * sheet.getFirstRowNum();//获取第一行的行号下标
			 * sheet.getLastRowNum();//获取最后一行的行号下标,遍历时需要加1
			 * row.getFirstCellNum();//获取某行的第一个单元格的下标
			 * row.getLastCellNum();//获取某行的最后一个单元格数量
			 */
//			for(int i=sheet.getFirstRowNum();i<sheet.getLastRowNum()+1;i++){
//				Row row = sheet.getRow(i);
//				for(int j=row.getFirstCellNum();j<row.getLastCellNum();j++){
//					Cell cell = row.getCell(j);
//				}
//			}

			Iterator<Row> iterator = sheet.iterator();
			while (iterator.hasNext()) {//遍历每一行的数据
				Row row = iterator.next();
				System.out.println("FirstRow="+sheet.getFirstRowNum()
						+",LastRow="+sheet.getLastRowNum()
						+",FirstCell="+row.getFirstCellNum()
						+",LastCell="+row.getLastCellNum()
						+",RowNum="+row.getRowNum());
				Iterator<Cell> cellIterator = row.cellIterator();
				while (cellIterator.hasNext()) {//遍历某行的所有单元格
					Cell cell =  cellIterator.next();
					switch (cell.getCellType()) {
					case Cell.CELL_TYPE_STRING://String类型
						System.out.println("String："+cell.getStringCellValue());
						try {
							System.out.println(formatSS.parse(cell.getStringCellValue()));//读取字符串格式的日期
						} catch (Exception e) {
						}
						break;
					case Cell.CELL_TYPE_BOOLEAN://Booble类型
						System.out.println("Boolean:"+cell.getBooleanCellValue());
						break;
					case Cell.CELL_TYPE_NUMERIC://Double类型
	                    if (DateUtil.isCellDateFormatted(cell)||cell.getCellStyle().getDataFormat() == 164) {//读取Double类型的日期
	                    	System.out.println("Data:————————》"+formatSS.format(cell.getDateCellValue()));
	                    } else {
	                        System.out.println("Double:"+cell.getNumericCellValue());
	                    }
						break;
					case Cell.CELL_TYPE_FORMULA:
						System.out.println("formula:"+formatSS.format(cell.getDateCellValue()));
						break;
					case Cell.CELL_TYPE_BLANK://空白的单元格
						System.out.println("Blank");
						break;
					default:
						break;
					}
				}
			}

		}catch (Exception e) {
			e.printStackTrace();
		}
	}
```
##4、读取Excel的数据并转换为Java Bean的集合

```
	public static List<Student> readExcel(File excelFile) {
		List<Student>list=new ArrayList<>();
		try {
			Workbook workbook=WorkbookFactory.create(excelFile);
			Sheet sheet = workbook.getSheetAt(0);
			for (int i = 0; i < sheet.getLastRowNum()+1; i++) {//遍历所有的行
				Row row = sheet.getRow(i);
				if (row==null||i==0||i>sheet.getLastRowNum()-2) {//舍弃表头和末尾
					continue;
				}
				Cell cell = row.getCell(0);
				String id = null;
				if (cell!=null) {
					id=cell.getStringCellValue();
				}
				cell=row.getCell(1);
				String name = null;
				if (cell!=null) {
					name=cell.getStringCellValue();
				}
				cell=row.getCell(2);
				boolean sex = false;
				if (cell!=null) {
					sex=cell.getBooleanCellValue();
				}
				int age = 0;
				cell=row.getCell(3);
				if (cell!=null) {
					age=(int) cell.getNumericCellValue();
				}
				double score = 0;
				cell=row.getCell(4);
				if (cell!=null) {
					score=cell.getNumericCellValue();
				}
				Date date = null;
				cell=row.getCell(5);
				if (cell!=null&&(DateUtil.isCellDateFormatted(cell)||cell.getCellStyle().getDataFormat() == 164)) {//读取Double类型的日期
					date=cell.getDateCellValue();
				}
				Student student=new Student(id, name, sex, age, score, date);
				list.add(student);			}
		} catch (Exception e) {
			e.printStackTrace();
		}
		return list;
	}

```
读取结果
```
Student [id=0000000, name=name0, sex=true, age=10, score=750.0, date=Thu Jan 25 14:25:07 CST 2018]
  ...
Student [id=00000029, name=name29, sex=false, age=39, score=721.0, date=Thu Jan 25 14:25:07 CST 2018]

```
Student类很简单字段有
```
	private String id;
	private String name;
	private boolean sex;// true = 男，
	private int age;
	private double score;
	private Date date;
```
##5、测试代码
```
		String sheetName="表1";
		String []sheetHeads={"学号","姓名","性别","年龄","成绩","日期"};
		List<Student> listContent=new  ArrayList<Student>();
		for (int i = 0; i < 30; i++) {
			listContent.add(new Student("000000"+i, "name"+i, i%2==0?true:false, i+10, 750-i,new Date() ));
		}
		FileOutputStream fos = null;
		try {
			fos = new FileOutputStream(new File("C:\\Users\\yuxue\\Desktop","student.xls"));
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		}
		Excel.writeExcel(sheetName, sheetHeads, listContent, fos);
		List<Student> list = Excel.readExcel(new File("C:\\Users\\yuxue\\Desktop","student.xls"));
		for (Student student : list) {
			System.out.println(student);
		}
```
我的CSDN博客：http://blog.csdn.net/wo_ha/article/details/79161616