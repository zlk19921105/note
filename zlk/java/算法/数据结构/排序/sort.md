## 排序算法

### 1 选择排序

n个记录的直接选择排序可经过n-1趟直接选择排序得到有序结果。（有序集 || 无序集） --不稳定（值相同时会出现）o(n^2)

**描述**：首先在未排序序列找到一个最小值，并将其存放在排序序列的第一个位置。接着从未排序序列中找到一个最小值将其放入排序序列的末尾。依次类推，直到序列排序完毕。（**当未排序序列中首位置不是最小时，将其与最小值的位置做交互，其余元素位置不变**）

例：3  , 5   ,2  , 4 ,  7

	第一次（3和2交换）     ：  2  || 5   3   4   7
	第二次（5和3交换）     ：  2  3  ||  5   4   7
	第三次（5和4交换）     ：  2  3  4   ||  5   7
	第四次（5最小，不交换） ：  2  3  4   5  ||   7

	注：剩余最后一个时，肯定最大，不需要再比。（||前为排序序列，||后为未排序）
	

**算法实现**：
	
需要比较的学生实体

	/**学生类
	 * @author  zhoulk
	 * date: 2019/5/17 0017
	 */
	@Getter
	@Setter
	public class Student implements Comparable{
	    /**姓名*/
	    private String name;
	    /**年龄*/
	    private Integer age;
	
	    public Student(String name, Integer age) {
	        this.name = name;
	        this.age = age;
	    }
	
	    @Override
	    public String toString() {
	        return "Student{" +
	                "name='" + name + '\'' +
	                ", age=" + age +
	                '}';
	    }
	
	    @Override
	    public int compareTo(Object o) {
	        if(this.age.compareTo(((Student)o).age)>0){
	            return 1;
	        }
	        if(this.age.compareTo(((Student)o).age)==0){
	            if(this.name.compareTo(((Student)o).name)>0){
	                return 1;
	            }
	        }
	        return 0;
	    }
	}

比较类

	/**desc:选择排序
	 * @author  zhoulk
	 * date: 2019/5/16 0016
	 */
	public class SelectSort<T> {
	
	    public static void main(String[] args) {
	        Integer[] arr = new Integer[]{1,4,5,2,1,8,13,6,9};
	        SelectSort<Integer[]> selectSort = new SelectSort();
	        Integer[] returnArr = selectSort.sortAll(arr);
	
	        System.out.println("直接使用引用对象-----------------------");
	        for (Integer v:arr) {
	            System.out.println(v+",");
	        }

	        System.out.println("使用泛型的返回值-----------------------");
	        for (Integer v:returnArr) {
	            System.out.println(v+",");
	        }

	        System.out.println("对象比较-----------------------");
	        Student[] students = { new Student("Zhang", 20),
	                new Student("Li", 23),
	                new Student("Wang", 50),
	                new Student("Liu", 17),
	                new Student("Aiu", 17),
	                new Student("Ma", 19)
	        };
	         selectSort.sortAll(students);
	        for (Student v:students) {
	            System.out.println(v+",");
	        }
	    }
	
	    /**
	     * 选择排序（从小到大）
	     * @param arr 需要排序数组
	     * @param <T> 结果
	     * @return
	     */
	    public <T extends Comparable<T>> T[]  sortAll(T[] arr){
	        if(arr==null||arr.length==0){
	           return null;
	        }
	
			//最后一次不需要再比
	        for(int i = 0;i<arr.length-1;i++){
               //记录未排序元素最小值下标
	           int minIndex = i;
	           for(int j = i+1;j<arr.length;j++){
	                if(arr[minIndex].compareTo(arr[j])>0){
	                    minIndex = j;
	                }
	           }
	
	            T temp = arr[i];
	            arr[i] = arr[minIndex];
	            arr[minIndex] = temp;
	        }
	        return arr;
	    }
	
	}


### 2 插入排序

n个记录的直接选择排序可经过n-1趟直接选择排序得到有序结果。 o(n^2)   --（稳定）

**描述：**默认第一个位置为有序，从第二个位置开始。每次取当前位置元素和前面元素相比。 如果当前元素比前面元素小，则交换位置。如果当前元素大于或者等于前一个元素则插入位置。 依次类推直到元素序列完全有序。(**插入时，前面元素是有序的**) 


例：6  , 5   ,3  , 4 ,  7  （ || 后面第一个元素是下一次需要插入的元素）

	第一次（6和5交换）     			 ：  5   6   ||   3    4   7
	第二次（6和3交换，后5后3交换）     ：  3   5   6   ||    4   7
	第三次（4和6交换，后4后5交换）     ：  3   4   5    6   ||   7
	第四次（7不变）    			  ：  3   4   5    6    7   ||


**算法实现**：

	/**插入排序
	 * @author  zhoulk
	 * date: 2019/5/20 0020
	 */
	public class InsertSort {
	
	    public static void main(String[] args) {
	        Integer[] array = RandomArray.getArray(1, 1000, 5000);
	        SelectSort<Integer[]> selectSort = new SelectSort();
	
	        //选择排序
	      /*  Long startTime = System.currentTimeMillis();
	        Integer[] returnArr = selectSort.sortAll(array);
	        Long endTime = System.currentTimeMillis();
	        System.out.println("花费时间："+(float)(endTime-startTime)/1000+"秒");*/
	
	        //插入排序
	        sort(array);
	        //sortOptimize(array);
	        for (Integer v:array) {
	            System.out.println(v+",");
	        }
	        /*Long startTime1 = System.currentTimeMillis();
	        Integer[] returnArr1 = selectSort.sortAll(array);
	        Long endTime1 = System.currentTimeMillis();
	        System.out.println("花费时间："+(float)(endTime1-startTime1)/1000+"秒");*/
	    }
	
	    /**
	     * 插入排序优化（小到大） -- 找到具体位置后再直接交换，需要靠后的数据顺移
	     * @param arr 排序数组
	     */
	    public static void sortOptimize(Integer[] arr){
	        for (int i = 1; i <arr.length ; i++) {
	            //记录当前值需要插入的位
	            int temp = arr[i] ;
	            int j;
	            for(j = i;j >0&&arr[j-1]>temp;j--){
	                arr[j] = arr[j-1];
	            }
	            arr[j] = temp;
	        }
	    }
	
	    /**
	     * 插入排序（小到大）--需要多次交换位置
	     * @param arr 排序数组
	     */
	    public static void sort(Integer[] arr){
			//首位有序
	        for (int i = 1; i <arr.length ; i++) {
	            int temp;
	            for(int j = i;j >0&&arr[j]<arr[j-1];j--){
	                temp = arr[j];
	                arr[j] = arr[j-1];
	                arr[j-1] = temp;
	            }
	        }
	    }
	}


### 3 冒泡排序

o(n^2)   --（稳定）

例：6  , 5   ,3  , 4 ,  7  （ || 后面第一个元素是下一次需要插入的元素）

	第一次     ：  5   3   4    6    ||   7
	第二次     ：  3   4   5   ||    6    7
	第三次     ：  3   4   ||   5    6    7
	第四次     ：  3   ||   4   5    6    7  
	第四次     ： ||   3    4   5    6    7  


**描述：**比较相邻的元素。如果第一个比第二个大，就交换他们两个。

对每一对相邻元素做同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。

针对所有的元素重复以上的步骤，除了最后一个。

持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。