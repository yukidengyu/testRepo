> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/JZJZ73/article/details/109297420)

  批量创建 txt 文件，并统一命名，在所有文件中写入要求的语句。

```
#文件个数
num=5
 
def main():
    #txt文件的存放路径
    desktop_path = "C:\\Users\\Zhang\\Desktop\\aaa\\"
    count=0
    for i in range(num):
 
        #命名格式
        full_path = desktop_path + "blxw_" + str(count) + '.txt'
        file = open(full_path, 'w')
 
        #写入的语句
        file.write("hello world!")
        print(full_path)
        count+=1
 
if __name__ == '__main__':
    main()
```