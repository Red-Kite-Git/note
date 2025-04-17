1.冒泡 升降序排列

```JAVA
for (int i = 0; i < Arrays.length - 1; i++) {
        for (int j = 0; j < Arrays.length - 1 - i; j++) {
            if (Arrays[j] > / < Arrays[j + 1]) {
            // >为升序,<为降序
            int temp = Arrays[j];
            Arrays[j] = Arrays[j + 1];
            Arrays[j + 1] = temp;
        }
    }
}
```



2.Arrays类中的方法

Arrays.sort(Arrays);                返回值：void        升序排列数组中的所有元素
Arrays.equals(Arrays1,Arrays2);     返回值：boolean     比较两个数组的内容，顺序等是否完全一样
Arrays.toString(Arrays);            返回值：String      把数组转化为字符串，可以快速在控制台查看数组内容
Arrays.fill(Arrays,val);            返回值：void        把数组中所有的元素全都赋值为val
Arrays.copyOf(Arrays,newLength);    返回值：Arrays[]    把数组复制出一个新的长度为newLength的数组，可以比原数组长度长
Arrays.binarySearch(Arrays,key);    返回值：int         从数组中查询key，存在则返回在数组中的元素下标，不存在则返回-1

*System.arraycopy(Arrays,srcPos,newArrays,destPos,length);
        //复制数组(数组1，起始位置，数组2，起始位置，元素个数)