---
title: POI Excel
tags: [Java]
created: '2019-01-22T14:07:36.688Z'
modified: '2019-03-19T06:29:18.678Z'
---

# POI Excel

## Excel 获取单元格的值
```java
float qs = getCellValue(sheet, 1, i, validateResult, Float.class);
```
```java
import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.ss.usermodel.Workbook;

/**
 * 从单元格中获取值指定类型的值
 *
 * @param sheet  Excel 表格
 * @param rowNum 行号
 * @param colNum 列号
 * @param result 验证结果
 * @param c      返回类型
 * @return 单元格中的数据
 */
private <T> T getCellValue(Sheet sheet, int rowNum, int colNum, Class<T> c, Set<String> result) throws Exception {
    // 1. 定义指定参数类型的构造函数
    // 2. 判断指定行是否存在，不存在默认返回 0
    // 3. 判断指定单元格是否存在，不存在默认返回 0
    // 4. 判断单元格格式是否正确，不存在默认返回 0
    // 5. 如果是整数去掉 POI 在末尾自动追加的小数，返回对应类型结果

    // [1] 定义指定参数类型的构造函数
    Constructor<T> constructor = c.getConstructor(String.class);

    // [2] 判断指定行是否存在，不存在返回默认值 0
    Row row = sheet.getRow(rowNum);
    if (row == null) {
        result.add("第 " + (rowNum + 1) + " 行没有数据但有格式，请在 Excel 中删除再重新导入<br>");
        return constructor.newInstance("0");
    }

    // [3] 判断指定单元格是否存在，不存在默认返回 0
    Cell cell = row.getCell(colNum);
    if (cell == null) {
        result.add("第 " + (rowNum + 1) + " 行" + "第 " + (char) (colNum + 65) + " 列没值<br>");
        return constructor.newInstance("0");
    }

    // [4] 判断单元格格式是否正确，不正确默认返回 0
    String value = cell.toString();
    if ((c.equals(Double.class) || c.equals(Integer.class) || c.equals(Float.class)) && !value.matches("[\\d.]+")){
        result.add("第 " + (rowNum + 1) + " 行" + "第 " + (char) (colNum + 65) + " 列格式不正确<br>");
        return constructor.newInstance("0");
    }

    // [5] 如果是整数去掉 POI 在末尾自动追加的小数，返回对应类型结果
    if (c.equals(Integer.class)) {
        value = value.split("\\.")[0];
    }

    return constructor.newInstance(value);
}
```
