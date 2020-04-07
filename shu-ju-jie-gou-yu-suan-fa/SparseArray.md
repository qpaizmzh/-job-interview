# 稀疏数组

## 用途

- 用于多数无效的数组数据，只有少部分的有效数据，做法是将其压缩成保存坐标和值的数组，节省空间

## 例子

- 需求：

  ![image-20200327135928062](..\image-20200327135928062.png)

- 代码实现：

  ```java
  
  package org.javaboy.vhr;
  import org.apache.logging.log4j.util.Strings;
  import java.io.*;
  import java.util.ArrayList;
  import java.util.List;
  
  public class SpareArray {
      public static void main(String[] args) {
          //二维数组
          int[][] spareArray = new int[11][11];
          spareArray[4][2] = 1;
          spareArray[5][3] = 2;
          spareArray[4][9] = 3;
  
          //寻找有效数据
          int count = 0;
          for (int[] array : spareArray) {
              for (int num : array) {
                  if (num != 0) {
                      count++;
                  }
              }
          }
          //压缩成压缩数组
          int[][] compressArray = new int[count + 1][3];
          compressArray[0][0] = spareArray.length;
          compressArray[0][1] = spareArray[0].length;
          compressArray[0][2] = count;
  
          //再次遍历一次原来的二维数组，把指定的值复制到压缩数组上
          //寻找有效数据
          int sum = 0;
          for (int i = 0; i < spareArray.length; i++) {
              for (int j = 0; j < spareArray[i].length; j++) {
                  if (spareArray[i][j] != 0) {
                      sum++;
                      compressArray[sum][0] = i;
                      compressArray[sum][1] = j;
                      compressArray[sum][2] = spareArray[i][j];
                  }
              }
          }
  
          //显示压缩后的数组,并存档到文件
          try {
              BufferedWriter bw = new BufferedWriter(new FileWriter(new File("map.txt")));
              for (int i = 0; i < compressArray.length; i++) {
                  for (int num : compressArray[i]) {
                      //System.out.printf("%d\t%d\t%d\n", array[0], array[1], array[2]);
                      //存档到文件
                      String numStr = Integer.toString(num);
                      numStr += " ";
                      bw.write(numStr);
                  }
                  bw.newLine();
                  bw.flush();
              }
              bw.close();
  
          } catch (IOException e) {
              e.printStackTrace();
          }
  
          //取出文档，并获取压缩数组
          String line = null;
          List<int[]> lists = new ArrayList<>();
          try {
              BufferedReader bufferedReader = new BufferedReader(new FileReader(new File("map.txt")));
              while ((line = bufferedReader.readLine()) != null) {
                  String[] strings = line.split(" ");
                  int[] ints = new int[3];
                  ints[0] = Integer.parseInt(strings[0]);
                  ints[1] = Integer.parseInt(strings[1]);
                  ints[2] = Integer.parseInt(strings[2]);
                  lists.add(ints);
              }
          } catch (FileNotFoundException e) {
              e.printStackTrace();
          } catch (IOException e) {
              e.printStackTrace();
          }
  
          int[][] aa = lists.toArray(new int[lists.size()][3]);
  
          //压缩数组还原二维数组
          int[][] spareArray2 = new int[aa[0][0]][aa[0][1]];
          for (int i = 1; i < compressArray.length; i++) {
              spareArray2[aa[i][0]][aa[i][1]] = aa[i][2];
          }
  
          for (int[] ints : spareArray2) {
              for (int num : ints) {
                  System.out.printf("%d\t", num);
              }
              System.out.println();
          }
      }
  }
  ```

  

  