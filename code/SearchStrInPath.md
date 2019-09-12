## SearchStrInPath 在文件夹中查找文件内指定字符串

- 具体实现：

~~~

import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

/**
 * @author wyz
 * @date 2019/9/12 11:41
 */

public class SearchStrInPath {
    public static int count = 0;

    /**
     * 要查找的文件目录
     */
    private static final String FILE_NAME = "E:\\test";

    /**
     * 要查找的字符串
     */
    private static final String TARGET_STRING = "es";


    public static void main(String[] args) {
        // 创建一个 File 实例，表示路径名是指定路径参数的文件
        File file = new File(FILE_NAME);
        String[] str = new String[]{TARGET_STRING};
        for (int i = 0; i < str.length; i++) {
            findFile(file, str[i]);
            print(str[i]);
        }

    }

    /**
     * 判断传入的是不是文件
     * @param file
     * @return
     */
    public static boolean isTrueFile(File file) {
        // 文件不存在或文件不可读
        if (!file.exists() || !file.canRead()) {
            return false;
        }
        /*
        // 文件以.开头
        if (file.getName().startsWith(".")) {
            return false;
        }
        // 文件以.结尾
        if (file.getName().endsWith(".")) {
            return false;
        }
        */
        return true;
    }

    public static void findFile(File file, String word) {
        File[] listFiles = file.listFiles();
        // 得到一个File数组，它默认是按文件最后修改日期排序的
        for (int i = 0; i < listFiles.length; i++) {
            // 判断是不是目录, 即目标目录下还有文件夹的话
            if (listFiles[i].isDirectory()) {
                // 调用本方法, 递归
                findFile(listFiles[i], word);
                // 判断传入的是不是的文件
            } else if (isTrueFile(listFiles[i])) {
                // 在文件中查找关键字
                search(listFiles[i], word);
            }
        }
    }

    /**
     * 在文件中查找关键字
     * @param file
     * @param word
     */
    public static void search(File file, String word) {
        try {
            int j = 0, k = 0, ch = 0;
            String str = null;
            FileReader fileReader = new FileReader(file);
            while ((ch = fileReader.read()) != -1) {
                str += (char) ch;
            }
            if (str != null) {
                while (str.indexOf(word, j) != -1) {
                    k++;
                    // 返回第一次出现的指定子字符串在此字符串中的索引
                    j = str.indexOf(word, j) + 1;
                }
            }
            if (k > 0) {
                System.out.println("在" + file.getAbsolutePath() + "有    " + k + " 个关键字" + word);
                count++;
            }
            fileReader.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void print(String word) {
        if (count != 0) {
            System.out.println("一共找到    " + count + " 个文件包含关键字" + word + "! \n");
            count = 0;
        } else {
            System.out.println("没有找到相应的文件");
        }
    }
}

~~~

- 文件结构

~~~
文件结构如下：
E:.
│  test - 副本 - 副本.txt	/test
│  test - 副本.txt		 /test
│  test.txt				  /test
│  tree.txt				  /test
│  
└─test
        .s.txt		      /testtest
        test - 副本.txt    /test
        test.txt          /test
~~~

- 输出如下：

~~~

在E:\test\test\.s.txt有    2 个关键字es
在E:\test\test\test - 副本.txt有    1 个关键字es
在E:\test\test\test.txt有    1 个关键字es
在E:\test\test - 副本 - 副本.txt有    1 个关键字es
在E:\test\test - 副本.txt有    1 个关键字es
在E:\test\test.txt有    1 个关键字es
一共找到    6 个文件包含关键字es! 

~~~

