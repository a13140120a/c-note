# c_plus_plus

*編譯器請使用 g++，gcc 不會使用 C++ standard library*
*ANSI encode, UTF-8 decode*
*繁體中文的 ANSI 是CP950*

* ## [inline](#001) #
* ## [變數等級](#002) #
* ## [函數多載](#003) #
* ## [input and output](#004) #
* ## [字串](#005) #
* ## [new & delete](#006) #
* ## [enum](#007) #
* ## [運算子多載](#008) #
* ## [類別繼承](#009) #
* ## [虛擬函數 & 抽象類別](#010) #
* ## [檔案處理](#011) #
* ## [try & catch](#012) #
* ## [名稱空間](#012) #
* ## [多個檔案(含標頭檔)](#014) #



****

<h1 id="001">inline</h1> 

* 語法:
  * `inline 回傳型態 func(void argv);`
  * 編譯器遇到`inline`會直接把程式碼展開(不會有stack的整理)，速度較快，但即便加入inline想要使用內嵌函數，編譯時也不一定就會實作
  * [參考資料](https://dotblogs.com.tw/v6610688/2013/11/27/introduction_inline_function)

<h1 id="002">變數等級</h1> 

cstdlib实现了stdlib.h中的所有功能，不过是按照C++的方式写的，所以与C++语言可以更好的配合。
```c
auto i;
exter char ch;
static float f;
```
* auto: [auto](https://blog.gtwang.org/programming/cpp-auto-variable-tutorial/)
* [extern](https://github.com/a13140120a/c_note/edit/main/README.md#extern--ifdef-1)
* [static](https://github.com/a13140120a/c_note/edit/main/README.md#%E9%9D%9C%E6%85%8B%E8%AE%8A%E6%95%B8)
  * #### static 外部變數: 
  * ```c
      #include <iostream>
      #include <cstdlib>          // cstdlib實現了stdlib.h中的所有功能，不過是按照c++的方式寫的，包括 malloc，free 等等
      using namespace std;
      static int a;               // 定義靜態外部整數變數a
      void odd(void);    	        // 函數原型的宣告 
      int main(void)
      {   
         odd();	      	        // 呼叫odd()函數
         cout << "after odd(), a=" << a << endl;
         system("pause");
         return 0;
      }

      void odd(void)		        // 自訂函數odd()，判斷a為奇數或是偶數
      {
         a=10;
         if(a%2==1)
            cout << "a=" << a << ", a是奇數" << endl;   // 印出a為奇數
         else
            cout << "a=" << a << ", a是偶數" << endl;   // 印出a為偶數
         return;
      }
    ```
* 暫存器變數:
  * 利用暫存器儲存變數，但如果 cpu 忙碌的話會把控制權還給 cpu ，然後把變數當成普通的區域變數處理
  * ```c
      #include <iostream>
      #include <cstdlib>
      #include <ctime>
      #include <iomanip>
      using namespace std;
      int main(void)
      {
         time_t start,end;
         register int i,j;            // 定義暫存器整數變數i與j
         start=time(NULL);	        // 記錄開始時間
         for(i=1;i<=50;i++)
         {
            for(j=1;j<=50;j++)
            {   
               cout << setw(2) << i << "*" << setw(2) << j;
               cout << "=" << setw(4) << i*j << "\t";
            }
            cout << endl;
         }
         end=time(NULL);	            // 記錄結束時間
         cout << "It spends " << difftime(end,start) << " seconds";
         system("pause");
         return 0;
      }
    ```



<h1 id="003">函數多載</h1> 

* [函數](https://github.com/a13140120a/c_note/edit/main/README.md#004)
* 沒有預設值的引數要放在引數列的左邊，有預設值的引數要放在沒有設值的引數的右邊
* `void func(int, double, int n=3, char ch='a')`
  * 並且省略傳遞引數時，只能從最右邊開始減少:
  * `func(5, 1.9);`，合法
  * `func(8, 6.3, 4);`，合法
  * `func(4, 3.7, 9, 'a');`，合法
  * `func();`，不合法
  * `func(6);`，不合法
  * `func(2, 1.9, 'b');`，不合法
* 多載範例:
  * ```c
      using namespace std;
      void print(void);                  // 以多載的方式宣告函數原型
      void print(int);
      void print(char,int);
      int main(void)
      {
         cout << "calling print(), ";
         print();
         cout << "calling print(8), ";
         print(8);
         cout << "calling print('+',3), ";
         print('+',3);
         system("pause");
         return 0;
      }

      void print(void)   			       // 沒有引數的print()，印出5個*
      {
         print(5);					   // 呼叫26~33行的print()，並傳入整數5
         return;
      }

      void print(int a)			       // 有一個引數的print()，印出a個*
      {
         int i;
         for(i=0;i<a;i++)
            cout << "*";
         cout << endl;
         return; 
      }

      void print(char ch,int a)		   // 有二個引數的print()，印出a個ch
      {
         int i;
         for(i=0;i<a;i++)
            cout << ch;
         cout << endl; 
         return;
      }
    ```

<h1 id="004">input and output</h1> 


* 以下例子如果輸入Alice Wu，只會出現 Alice，因為 cin 讀取到空白或是 Enter 鍵就會結束。
  * ```c
      #include <iostream>
      #include <cstdlib>
      using namespace std;
      int main(void)
      {
         char name[15];
         int i;
         for(i=0;i<2;i++)
         {
            cout << "What's your name? ";
            cin >> name;         // 以cin輸入字串
            cout << "Hi, " << name << ", how are you?" << endl << endl;
         }
         system("pause");
         return 0;
      }
    ```
* 可以改成 `cin.getline(字串名稱, 最大字串長度, 字串結束字元)`，其中字串結束字元欲設為 `\n`
  * `cin.get(ch);`可以接收輸入的一個字元
  * ```c
      #include <iostream>
      #include <cstdlib>
      using namespace std;
      int main(void)
      {
         int age;
         char name[20];
         cout << "How old are you? ";
         cin >> age;
         cout << "What's your name? ";
         cin.getline(name,20);
         cout << name << " is " << age << "-years-old!" << endl;
         system("pause");
         return 0;
      }
    ```

<h1 id="005">字串</h1> 

* 標頭檔：`#include <string>`
* string 不需要結束字元
* 定義與宣告:
  * `string str1 = "hello world";`
  * `string str1("hello world");`
  * `string str2(str1);`
  * `string str2(6, 's');`: 印出6個s
* 取長度:
  * `str2.length()`
* 運算:
  * `+`、`-`、`+=`、`==`、`!=`
  * `>`、`>=`、`<=`：兩個 string 逐字元比較，直到字元不同時，比較 ASCII 值大小
* 成員函數：
  * `str1.assign(str2)`：將str2 存放到str1，若str1原本就有值會被覆蓋
  * `str1.assign(str2, index, length)`：將str2 的第 inedx 個字元開始，長度 length 存放到 str1。
  * `str1.at(index)`：從 str1 取出第 index 個字元，若 index 超過字串長度，則終止動作
  * `str1.append(str2)`：將 str2 附加在 str1 之後。
  * `str1.append(str2, index, length)`：將 str2 的第 index 個字元開始，長度 length 附加在 str1 之後。
  * `str1.erase(index, length)`：刪除從 str1 的 index 開始後 length 個字元
  * `str1.find(str2)`：從 str1 中找尋是否包含 str2 並返回 str2 在 str1 的位置
  * `str1.find(str2, index)`：從 str1 中的第 index 個位置找尋是否包含 str2 並返回 str2 在 str1 的位置
  * `str1.insert(index, str2)`：從 str1 的 index 個位置插入 str2
  * `str1.substr(index)`：取出從 str1 的 index 個位置到結束
  * `str1.substr(index, length)`：取出從 str1 的 index 個位置 length 長度的字串
  * `str1.maxsize()`：返回字串可以達到的最大長度。
  * `str1.empty()`：是否為空字串，若是返回1，若否返回0
  * `str1.clear()`：清除字串內容
  * `str1.swap(str2)`：交換字串內容
  * `str1.compare(str2)`：相同回傳0，不同回傳1
  * `str1.compare(str1_index, str1_length, str2, str2_index, str2_length)`：子字串 ASCII 比較，相等傳回0，str1 小於 str2 回傳負值，str1 大於 str2 回傳正值。
  * `str1.replace(index, length, str2)`：將 str1 的第 index 開始，長度 length 替換成 str2



<h1 id="006">new & delete</h1> 

* new 
  * 定義與宣告：
  * ```c
      型態A *指標變數名稱B;
      指標變數名稱B = new 型態A;
    ```
  * 或者：`型態A *指標變數名稱B = new 型態A;`
  * 例如：`int *p = new int;`
  * 或者：`int *p = new int(100);`，分配記憶體空間，並且填入100這個初始值。(c++11以後才允許初始化)
  * new 一個字串：
    * ```c
        char *text = "hello world";
        ptr = new char[strlen(text)+1] // \0字元
        strcpy(ptr, text)
      ```
  * 宣告陣列：
    * ```c
        型態A *指標變數名稱B;
        指標變數名稱B = new 型態A[個數];
      ```
    * 或者：`型態A *指標變數名稱B = new 型態A[個數];`
    * 二維陣列：
      * 二維陣列就是多個一為陣列，new 2個一維陣列：`int **arr = new int*[2];`
      * 現在 arr[0]、arr[1] 可以分別儲存一維陣列位址，目前尚未初始，若每段一維陣列的長度是 3，可以如下動態配置，並將一維陣列每個元素初始設為 0 ：
      * ```c
          for(int i = 0; i < 2; i++) {
              arr[i] = new int[3]{0};
          }
        ```
  * 帶位置的 new：
    * 使用的其中一個原因例如：需要把物件放在特定硬體的記憶體位址上，或者放在多處理器核心的共享的記憶體位址上。
    * 範例：
    * ```c
        #include <iostream>
        using namespace std;

        int main()
        {
            char buf[100];
            int *p=new (buf) int(101); // 宣告一個指標 p 並指向 buf
            cout<< *buf <<endl;
            cout<< *(int*)buf <<endl;  
            cout<< *buf <<endl;  // 直接印出 buf[0] 沒有轉成 int 就是 e
            cout<< buf <<endl;   // 印出整個 buf 直到遇到 \0

            /* 當更改 p 的值時，buf 內的值也會修改 */
            *p = 102;  
            cout<< *(int*)buf <<endl;  
            /* 宣告 int[10] 的陣列放在  buf 裡面 */
            p2 = new (buf) int[10];

            return 0;
        }
    * ```
* delete：
  * `delete 變數名稱`
  * 釋放整個陣列：`delete 變數名稱[]`，中括號不用填數字。
  * 記得把`ptr=NULL;`

<h1 id="007">enum</h1> 

* [enum](https://github.com/a13140120a/c_note/edit/main/README.md#008)
* c++ 設值：
  * `enum型態變數名 = static_cast<型態變數名>(值)`
  * ```c
    enum color{
        red,  // 宣告的同時會把red、green、blue 定義為常數 0、1、2
        green,
        blue // 也可以在宣告的同時指定 blue 為 5
    }shirt;
    /* 在 C語言當中是合法的，但 C++ 是不合法的 */
    // shirt=3;
    /* C++ 的作法 */
    shirt = static_cast<color>(2); // 如果填入 color 內部以外的值，有些編譯器是不合法的
    ```

<h1 id="008">class</h1> 

  * ## [class宣告與定義](#0081) #
  * ## [以物件為引數](#0082) #
  * ## [this 指標](#0083) #
  * ## [類別函數多載](#0084) #
  * ## [友誼函數](#0085) #
  * ## [建構元(建構子)](#0086) #
  * ## [靜態成員](#0087) #
  * ## [物件指標](#0088) #
  * ## [解構元(解構子)](#0089) #
  * ## [拷貝建構子(元)](#00810) #



<h2 id="0081">class宣告與定義</h2> 

* 宣告：
  * ```c
      class ClassA  // 通常類別的第一個字會大寫
      {
          private:
              char secret;  // 只有內部成員可以存取
          public:
              char id;
              int a;
              int b;
              
          int class_func()
          {
              cout << "do something" << endl;
              return a + b;
          }
          
          void call_class_func()
          {
              return class_func(); 
          }
          
      }; // 記得要分號
    ```
  * 上述類別的 size 為 12，因為編譯器會以內部最大的資料型態來為最小單位。
* 定義：
  * ```c
      ClassA classa;
      classa.id='a';
      classa.a = 0;
      classa.b = 1;
    ```

* 函數的位置：
  * ```c
      class CWin    			       // 定義視窗類別CWin
      {
         public:
           char id;
           int width;   
           int height;
           int area(void);    	   // 成員函數area()的原型
      };

      int CWin::area(void) 	       // 類別外定義area()函數
      {                 
         return width*height;
      }
      /*
      inline int CWin::area(void)   // 以 inline 的方式定義，功能跟一般的 inline 一樣。
      {                 
         return width*height;
      }
      */
      
      int main(void)
      {
         CWin win1;   			   // 宣告CWin類別型態的變數win1 

         win1.id='A';
         win1.width=50;	
         win1.height=40;

         cout << "Window " << win1.id << ":" << endl;
         cout << "Area = " << win1.area() << endl; 

         system("pause");
         return 0;
      }
    ```
    
<h2 id="0082">以物件為引數</h2>  
    
* ```c
    func(classA clsa)
    {
        cout << clsa.a << endl;
    }
  ```    
    

<h2 id="0083">this 指標</h2>  
    
* this 指標：
  * ```c
      class ClassA
      {
          public:
              char id;
              int a;
              int b;
              
          int class_func()
          {
              return this->a; // 回傳 a 的值
          }
      }; // 記得要分號
    ```

<h2 id="0084">類別函數多載</h2> 

* ```c
    class CWin    		                    // 定義視窗類別CWin
    {
       public:
         char id;
         int width;   
         int height;

         int area() 		                // 定義成員函數area(), 用來計算面積
         {                 
            return width*height;
         }
         void show_area(void)
         {
            cout << "Window " << id << ", area=" << area() << endl;
         }
         void set_data(char i,int w,int h)	// 第一個set_data()函數
         {
            id=i;         
            width=w; 	
            height=h; 
         }
         void set_data(char i)  			// 第二個set_data()函數
         {
            id=i;         
         } 
         void set_data(int w,int h)  		// 第三個set_data()函數
         {
            width=w; 	
            height=h;         
         }          
    };

    int main(void)
    {
       CWin win1,win2;   

       win1.set_data('A',50,40); 
       win2.set_data('B');
       win2.set_data(80,120);

       win1.show_area(); 
       win2.show_area();        

       system("pause");
       return 0;
    }
  ```
  
<h2 id="0085">友誼函數</h2> 
  

* 可以存取類別的私有成員
* 可以在類別內直接定義。也可以在類別內宣告，然後在類別外定義。
* ```c
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    class CWin                              // 定義視窗類別CWin
    {
       public:
         void set_data(char i,int w, int h) // 設定數值的函數 
         {
            id=i;
            width=w;
            height=h;
         }
       private:
         char id;
         int width;   
         int height;  

       friend void show_member(CWin);   	// 友誼函數的原型
    };

    void show_member(CWin w)  			    // 定義友誼函數
    {
        cout << "Window " << w.id;
        cout << ": width = " << w.width;
        cout << ", height = " << w.height << endl;
    } 

    int main(void)
    {
       CWin win1,win2;  

       win1.set_data('A',50,40);  			// 呼叫set_data()來設值 
       win2.set_data('B',80,60);
       show_member(win1);
       show_member(win2);

       system("pause");
       return 0;
    }
  ```

  
<h2 id="0086">建構元(建構子)</h2> 


* 定義建構子:
  ```C++
    #include <iostream>
    using namespace std;

    class Window
    {
    private:
      char id;
      int width, height;

    public:
      Window(char i, int w, int h)  // 建構子
      {
        id = i;
        width = w;
        height = h;
        cout << "呼叫建構元" << id << endl;
      }

      /*
      Window(char i, int w, int h): id(i), width(w), height(h)  // 另一種定義建構子的方法。
      {
          cout << "do something" << endl;
      }
      */

      void show_member(void)
      {
        cout << "window" << id << ":" << endl;
        cout << "width=" << width << endl << "height=" << height << endl;
      }
    };
    /*
    CWin::CWin(char i,int w,int h)   	// 建構元也可以定義在類別之外
    {
       id=i;
       width=w;
       height=h;
       cout << "CWin 建構元被呼叫了..." <<endl;
    }
    */ 

    int main(void)
    {
      Window win1('a', 50, 40);
      Window win2('b', 60, 70);

      win1.show_member();
      win2.show_member();

      system("pause");
      return 0;
    };

  ```

* 建構子多載：
  * ```c++
      using namespace std;
      class CWin                      // 定義視窗類別CWin
      {
         private:
           char id;
           int width, height;

         public:     
           CWin(char i,int w,int h)	// 有三個引數的建構元
           {
              id=i;
              width=w;
              height=h;
              cout << "CWin(char,int,int) 建構元被呼叫了..." << endl;
           }
           CWin(int w,int h)   		// 只有兩個引數的建構元
           {
              id='Z';
              width=w;
              height=h;
              cout << "CWin(int,int) 建構元被呼叫了..." << endl;        
           }        
           void show_member(void)  	// 成員函數，用來顯示資料成員的值
           {
              cout<< "Window " << id << ": ";
              cout<< "width=" << width << ", height=" << height << endl;
           }
      };

      int main(void)
      {
         CWin win1('A',50,40); 		// 建立win1物件，並呼叫三個引數的建構元
         CWin win2(80,120); 		    // 建立win2物件，並呼叫二個引數的建構元

         win1.show_member();  
         win2.show_member();

         system("pause");
         return 0;
      }

    ```
  * 呼叫類別時，同時也會呼叫建構子，
  * 若程式碼內沒有定義建構子的話，編譯器會自動產生並呼叫預設建構子，若程式碼內已經有定義建構子的話，則不會產生預設建構子 
  * ```c
     CWin win1('A',50,40); 		// 建立win1物件，並呼叫三個引數的建構元
     CWin win2(80,120); 		    // 建立win2物件，並呼叫二個引數的建構元
     CWin win3;             // 錯誤，因為若程式碼內已經有定義建構子，所以不會產生預設建構子，CWin()不存在
    ```
  * 如果以下兩種建構子同時存在，也會發生編譯錯誤：
  * ```c
      CWin(char i='D', int w=100, h=100) {}  // 每個成員都有預設值，因此可以用 CWin win1 呼叫
      /* 以及 */
      CWin() {}
    ```
  * 因為如果出現`CWin win1;`這樣的敘述的話，編譯器會不知道要呼叫上面哪一個


<h2 id="0087">靜態成員</h2> 


* 靜態成員函數不能存取物件成員，因為物件成員要有"物件"才會存在，但是類別不需要有"物件"就能存在，因此靜態成員函數這種依附於類別的東西不能存取物件的東西是很正常的。
* this 也是屬於物件的，所以靜態成員函數也不能使用 this
* ```c
    using namespace std;
    class CWin    			    // 定義視窗類別CWin
    {
       private:
         char id;
         int width, height;
         // static int num;  // 靜態成員也可以是 private

       public:  
         static int num;   	    // 將靜態資料成員num宣告為public
         CWin(char i,int w,int h):id(i),width(w),height(h)
         {
            num++;    			// 將靜態資料成員的值加1
         }
         CWin()
         {
            num++;    			// 將靜態資料成員的值加1
         }
         static void count(void)       // 靜態成員函數
         {
            cout << "已建立 " << num << " 個物件了..." << endl;
         }
    };

    int CWin::num=0;    		// 設定靜態資料成員num的初值

    int main(void)
    {
       CWin win1('A',50,40); 	
       CWin win2('B',60,80);
       cout << "已建立 " << CWin::num << " 個物件了..." << endl;

       CWin my_win[4];
       cout << "已建立 " << CWin::num << " 個物件了..." << endl;  

      /* 如果不是使用靜態成員函數就要這樣呼叫，非常奇怪 */
      // win1.count()
      // win2.count()
      CWin::count()  // 使用靜態函數成員

       system("pause");
       return 0;
    }
  ```

<h2 id="0088">物件指標</h2> 


* ```c
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    class CWin                          // 定義視窗類別CWin
    {
       private:
         char id;
         int width, height;

       public:     
         CWin(char i,int w,int h):id(i),width(w),height(h) // 建構元 
         {}

         void compare(CWin *win)        // 以指向物件的指標為引數
         {
            if(this->area() > win->area())
              cout << "Window " << this->id << " is larger" << endl;
            else
              cout << "Window " << win->id << " is larger" << endl;
         }
         int area(void)  			    // 成員函數area()
         {
            return width*height;	    // 傳回物件的面積值
         }  
    };

    int main(void)
    {
       CWin win1('A',70,80);		
       CWin win2('B',60,90); 			
       CWin *ptr1=&win1;                // 宣告ptr1指標，並將它指向物件win1
       CWin *ptr2=&win2;                // 宣告ptr2指標，並將它指向物件win2

       ptr1->compare(ptr2);             // 用ptr1呼叫compare()，並傳遞ptr2

       system("pause");
       return 0;
    }
  ```

<h2 id="0089">解構元(解構子)</h2> 



* ```c
    ~類別名稱()
    {
        /* do something */
        /* 解構元沒有回傳值 */
    }
  ```
* 範例：
* ```c
    #include <iostream>
    #include <cstdlib>
    #include <string>
    using namespace std;
    class CWin                      // 定義視窗類別CWin
    {
       private:
         char id, *title;          // 宣告title為指向字元陣列的指標	    

       public:     
         CWin(char i='D', char *text="Default window"):id(i)
         {
            title=new char[strlen(text)+1];       // 配置記憶體空間
            strcpy(title,text);
         }
         ~CWin()                    // 解構元的原型
         {
            cout << "解構元被呼叫了，Win " << this->id << "被銷毀了.." << endl;
            delete [] title;  	    // 釋放title所指向的記憶體空間
            system("pause");
         }
         void show(void)
         {
            cout << "Window " << id << ": " << title << endl;
         }
    };

    int main(void)
    {
       CWin win1('A',"Main window");
       CWin win2('B');

       win1.show(); 
       win2.show();
       cout << "sizeof(win1)= " << sizeof(win1) << endl;
       cout << "sizeof(win2)= " << sizeof(win2) << endl;  

       system("pause");
       return 0;
    }
  ```
* 使用 new 建立物件，程式結束時不會呼叫解構子：
* ```c
    #include <iostream>
    #include <cstdlib>
    using namespace std;

    class CWin                               // 定義視窗類別CWin
    {
       private:
         char id, *title;   	             // 宣告title為指向字元陣列的指標

       public:     
         CWin(char i='D', char *text="Default window"):id(i)
         {
            title=new char[strlen(text)+1];  // 配置記憶體空間
            strcpy(title,text);
         }
         ~CWin()                             // 解構元的原型
         {
            cout << "解構元被呼叫了，Win " << this->id << "被銷毀了.." << endl;
            delete [] title;  	             // 釋放title所指向的記憶體空間
            system("pause");
         }
         void show(void)
         {
            cout << "Window " << id << ": " << title << endl;
         }
    };

    int main(void)
    {
       CWin win1('A',"Main window"); 	
       CWin *ptr;  			                 // 宣告ptr為指向CWin物件的指標
       ptr=new CWin('B'); 	                 // 建立新的物件，並讓ptr指向它

       win1.show(); 			             // 以win1物件呼叫show()函數
       ptr->show();			                 // 以ptr指標呼叫show()函數

       system("pause");

       delete ptr; // 必須要加這行才會觸發 B 的解構子

       return 0;
    }
  ```
    
    
<h2 id="00810">拷貝建構子(元)</h2> 
    

* ```c
    CWin win1('A', 50, 40); // 建立 win1
    CWin win2(win2); // 建立 win2，其屬性成員與 win1 一模一樣
  ```
* 若無定義則編譯器會產生欲設拷貝建構子
* 自定義拷貝建構子：
* ```c
    類別名稱(const 類別名稱 &引數名)
    {
        /* do something */
    }
* ```
* 範例：
* ```c
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    class CWin                      // 定義視窗類別CWin
    {
       private:
         char id;
         int width, height;

       public:     
         CWin(char i,int w,int h):id(i),width(w),height(h)  
         {
            cout << "建構元被呼叫了..." << endl; 
         }
         CWin(const CWin &win)      // 定義拷貝建構元
         {
            cout << "拷貝建構元被呼叫了..." << endl; 
            id=win.id;
            width=win.width;   
            height=win.height; 
         }             
         void show_member(void)
         {
            cout << "Window " << id << ": ";
            cout << "width=" << width << ", height=" << height << endl;
         }
    };

    int main(void)
    {
       CWin win1('A',50,40); 	
       CWin win2(win1);             // 呼叫拷貝建構元 

       win1.show_member(); 
       win2.show_member();

       system("pause");
       return 0;
    }
  ```
* 常犯錯誤1，拷貝建構子與動態記憶體：
  * ```c
      class CWin                                 // 定義視窗類別CWin
      {
         private:
           char id, *title;

         public:     
           CWin(char i='D', char *text="Default window"):id(i)  
           {        
              cout << "建構元被呼叫了..." << endl; 
              title=new char[strlen(text)+1];      // 配置記憶空間
              strcpy(title,text);
           }
           CWin(const CWin &win)
           {
              cout<< "拷貝建構元被呼叫了..." <<endl; 
              id=win.id;
              // title = win.title;  // 使用這種方法 ptr2 的 title 會直接指向 ptr1 的 title
              /* 解決辦法 */
              title=new char[strlen(win.title)+1];     // 配置新記憶空間
              strcpy(title,win.title);         
           }             
           ~CWin()                                 // 解構元的原型 
           {
              delete [] title;
           }      
           void show(void)
           {
              cout << "Window " << id << ": " << title << endl;
           }
      };

      int main(void)
      {
         CWin *ptr1=new CWin('A',"Main window"); 	
         CWin *ptr2=new CWin(*ptr1);               // 以ptr1所指向的物件為初值建立新物件 

         ptr1->show(); 
         ptr2->show();

         delete ptr1;                              // 釋放ptr1所指向的記憶空間
         cout << "將ptr1所指向的物件刪除後..." << endl; 
         ptr2->show(); 

         delete ptr2;                              // 釋放ptr2所指向的記憶空間
         system("pause");
         return 0;
      }
  * ```

* 常犯錯誤2，物件引數：
  * ```c
      #include <iostream>
      #include <cstdlib>
      using namespace std;
      class CWin                                    // 定義視窗類別CWin
      {
         private:
           char id, *title;

         public: 
           CWin(char i='D', char *text="Defaule window"):id(i)  
           {
              cout << "建構元被呼叫了..." << endl;         
              title=new char[strlen(text)+1];       // 配置記憶空間
              strcpy(title,text);
           }
           ~CWin()                                  // 解構元的原型 
           {
              delete [] title;
           }      
           void show()
           {
              cout << "Window " << id << ": " << title << endl;
           }
      };

      void display(CWin win)                        // 用來呼叫CWin類別裡的show()函數
      {
         win.show();
      }

      int main(void)
      {
         CWin *ptr1=new CWin('A',"Main window"); 	

         display(*ptr1);  // 可以正常顯示
         display(*ptr1);   // 不能正常顯示
         /*
             因為呼叫 display 的時候 CWin 物件在引數傳遞時會被拷貝進去  display 裡面
             這時候會呼叫拷貝建構子，title 指向 CWin 的 title
             display 執行結束之後，因為 CWin 的拷貝物件是屬於區域變數，因此會被銷毀
             所以 title 的記憶體空間也會一起被釋放，連帶內容一起被清除
             所以第二次呼叫 display 的時候無法正常顯示
         */
         delete ptr1;

         system("pause");
         return 0;
      }

    ```


<h1 id="008">運算子多載</h1> 


* 範例-「>」多載：
  * ```c
      #include <iostream>
      #include <cstdlib>
      using namespace std;
      class CWin                                              // 定義視窗類別CWin
      {
         private:
           char id;
           int width, height;

         public:
           CWin(char i,int w,int h):id(i),width(w),height(h)  // 建構元
           {}
           int operator>(CWin &win)  // (win1>win2)
           {
              return(this->area() > win.area());
           }
           int operator>(const int &var)  // (win1>7000)
           {
              return(this->area() > var);
           }
           int area(void)
           {
              return width*height;
           }
      };

      int operator>(const int &var, CWin &win)  // 一般函數，多載 >，(4500>win2)
      {
          return( var > win.area());
      }

      int main(void)
      {
         CWin win1('A',70,80);
         CWin win2('B',60,70);

         if(win1>win2)                                        // 呼叫第一個operator>()函數
            cout << "win1 is larger than win2" << endl;
         else
            cout << "win1 is smaller than win2" << endl;

         if(win1>7000)                                        // 呼叫第二個operator>()函數
            cout << "win1 is larger than 7000" << endl;
         else
            cout << "win1 is smaller than 7000" << endl;

         if(4500>win2)                                        // 呼叫第三個operator>()函數
            cout << "win2 is smaller than 4500" << endl;
         else
            cout << "win2 is larger than 4500" << endl;

         system("pause");
         return 0;
      }

    ```
* 範例-「+」多載：
  * ```c
      #include <iostream>
      #include <cstdlib>
      using namespace std;
      class CWin                          // 定義視窗類別CWin
      {
         private:
           char id;
           int width, height;

         public:     
           CWin(char i='D',int w=10,int h=10):id(i),width(w),height(h)
           {}

           CWin operator+(CWin &win)      // 定義「+」運算子的多載
           {
              int w,h;
              w = this->width > win.width ? this->width : win.width;
              h = this->height > win.height ? this->height : win.height;
              return CWin('D',w,h);       // 呼叫建構元建立並傳回新的物件
           }
           void show_member(void)
           {
              cout << "Window " << id << ": ";
              cout << "width=" << width << ", height=" << height << endl;
           }
      };

      int main(void)
      {
         CWin win1('A',70,80);
         CWin win2('B',60,90); 
         CWin win3;

         win3=win1+win2;                  // 物件的加法運算
         win3.show_member();

         system("pause");
         return 0;
      }
    ```
* 常犯錯誤：沒有設定運算子「=」多載：
  * ```c
      #include <iostream>
      #include <cstdlib>
      using namespace std;
      class CWin                            // 定義視窗類別CWin
      {
         private:
           char id, *title;

         public:     
           CWin(char i='D', char *text="Default window"):id(i)  
           {
              title=new char[50];           // 配置可容納50個字元的記憶空間
              strcpy(title,text);           // 將text所指向的字串拷貝給title
           }
           void set_data(char i, char *text)
           {
              id=i;    
              strcpy(title,text);           // 將text所指向的字串拷貝給title
           }    
           void operator=(const CWin &win)  	// 定義設定運算子「=」的多載
           {
               id=win.id;
               strcpy(this->title,win.title);  // 字串拷貝
           } 
           void show(void)
           {
              cout << "Window " << id << ": " << title << endl;
           }

           ~CWin(){ delete [] title; }      // 解構元

           CWin(const CWin &win)  		  // 拷貝建構元
           {
              id=win.id;
              strcpy(title,win.title); 
           }     
      };

      int main(void)
      {
         CWin win1('A',"Main window"); 	
         CWin win2; 	 

         win1.show(); 
         win2.show();

         win1=win2;                         // 設定win1=win2
         cout << endl << "設定 win1=win2 之後..." << endl;
         win1.show();
         win2.show();

         win1.set_data('B',"Hello window");
         cout << endl << "更改 win1的資料成員之後..." << endl; 
         win1.show(); 
         win2.show();    // 如果沒有多載「=」的話，會跟著 win1 一起修改，因為「=」預設是直接把 win1.title = win2.win2.title

         system("pause");
         return 0;
      }
    ```
* 進階應用： w1=w2=w3
  * ```c
      #include <iostream>
      #include <cstdlib>
      using namespace std;
      class CWin    // 定義視窗類別CWin
      {
         private:
           char id, *title;

         public:     
           CWin(char i='D', char *text="Default window"):id(i)  
           {
              title=new char[50];
              strcpy(title,text);
           }
           void set_data(char i, char *text)
           {
              id=i;
              strcpy(title,text); 
           }   
           CWin &operator=(const CWin &win)     // 定義設定運算子「=」的多載
           {
              id=win.id;
              strcpy(this->title,win.title);    
              return *this;   
           }          
           void show(void)
           {
              cout<<"Window "<< id <<": "<< title <<endl;
           }

           ~CWin(){ delete [] title; } 

           CWin(const CWin &win)                // copy constructor
           {
              id=win.id;
              strcpy(title,win.title);         
           }     
      };

      int main(void)
      {
         CWin win1('A',"Main window"); 
         CWin win2('B',"Big window"); 
         CWin win3;

         win1.show(); 
         win2.show(); 
         win3.show(); 

         win1=win2=win3;                        // 設定win1=win2=win3
         win1.set_data('A',"Hello window");     // 修改win1的內容

         cout << "設定win1=win2=win3,並更改win1的成員之後 ..." << endl; 
         win1.show(); 
         win2.show();    
         win3.show();

         system("pause");
         return 0;
      }
    ```


<h1 id="009">類別繼承</h1> 

* 可以繼承的東西：
  * 資料成員
  * 成員函數
  * 除了「=」以外的多載運算子
* 不能繼承的東西：
  * 建構元
  * 解構元
  * 「=」的多載
* 指向父類別的指標可以轉成指向子類別
  * ```C
        CWin win('A',70,80);
        CMiniWin m_win('B',50,60);	// 建立子類別的物件

        CWin *ptr=NULL;   			// 宣告指向基底類別(父類別)的指標

        ptr=&win;					// 將ptr指向父類別的物件win
        ptr->show_area();			// 以ptr呼叫show_area()函數

        ptr=&m_win;				// 將ptr指向子類別的物件m_win
        ptr->show_area();	  		// 以ptr呼叫show_area()函數
      
  * ```
* 父類別的 private 子類別無法繼承
* protected 可以被子類別存取，但無法被外部存取
* 語法：
  * ```c
      class 父類別名稱
      {
          父類別成員
      }
      
      class 子類別名稱: 修飾子(modyfier，可為 public/private/protected) 父類別名稱
      {
          子類別成員
      }
  * ```
  * 若繼承的方式(modyfier)為 public，則原先父類別的 public 繼承到仔類別也是 public，而父類別的 protected 繼承到子類別也是 protected
  * 若繼承的方式(modyfier)為 protected，則原先父類別的 public 和 protected 繼承到子類別都是 protected
  * 若繼承的方式(modyfier)為 privaye，則原先父類別的 public 和 protected 繼承到子類別都是 private
  * 子類別與父類別有同樣的成員函數時，會被 override(改寫)
* 範例：
* ```c
    #include <iostream>
    #include <cstdlib>
    using namespace std;
    class CWin                          // 定義CWin類別，在此為父類別
    {
       private:
         char id;
         int width,height;

       public:
         CWin(char i='D',int w=10,int h=10):id(i),width(w),height(h)  
         {
            cout << "CWin()建構元被呼叫了..." << endl;
         }
         void show_member(void)  	    // 成員函數，用來顯示資料成員的值
         {
            cout << "Window " << id << ": ";
            cout << "width=" << width << ", height=" << height << endl;
         }
    };

    class CTextWin : public CWin        // 定義CTextWin類別，繼承自CWin類別
    {
       private:                  	    // 子類別裡的私有成員
          char text[20];

       public:     					    // 子類別裡的公有成員
          CTextWin(char *tx)     		// 子類別的建構元
          {
             cout << "CTextWin()建構元被呼叫了..." << endl; 
             strcpy(text,tx);
          }
          void show_text()			    // 子類別的成員函數
          {
             cout << "text = " << text << endl;
          }
    };

    int main(void)
    {
       CWin win('A',50,60); 	   		// 建立父類別的物件
       CTextWin txt("Hello C++");		// 建立子類別的物件

       win.show_member();  			    // 以父類別物件呼叫父類別的函數
       txt.show_member();				// 以子類別物件呼叫父類別的函數
       txt.show_text();				    // 以子類別物件呼叫子類別的函數

       cout << "win 物件佔了 " << sizeof(win) << " bytes" << endl;
       cout << "txt 物件佔了 " << sizeof(txt) << " bytes" << endl; 

       system("pause");
       return 0;
    }
  ```
* 如果沒有特別定義的話，編譯器預設會呼叫父類別的沒有引數的建構子`CWin()`
* 如果父類別有定義其他建構元，而沒有定義`CWin()`，則當子類別呼叫`CWin()`的時候會產生錯誤
* 呼叫父類別中特定的建構元：
  * ```c
      #include <iostream>
      #include <cstdlib>
      using namespace std;
      class CWin                              // 定義視窗類別CWin
      {
         private:
           char id;
           int width,height;

         public: 
           CWin(char i='D',int w=10,int h=10):id(i),width(w),height(h)  
           {
              cout << "CWin()建構元被呼叫了..." << endl;
           }
           CWin(int w,int h):width(w),height(h)  
           {
              cout << "CWin(int w,int h)建構元被呼叫了..." << endl;
              id='K';
           }     
           void show_member(void)  
           {
              cout << "Window " << id << ": ";
              cout << "width=" << width << ", height=" << height << endl;
           }
      };

      class CTextWin : public CWin
      {
         private:
            char text[20];

         public:
            CTextWin(int w,int h): CWin(w,h)  // CTextWin(int,int)建構元，繼承CWi(int w,int h)
            {
               cout << "CTextWin(int w,int h)建構元被呼叫了..." << endl; 
               strcpy(text,"Have a good night");
            }
            CTextWin(char *tx)                // CTextWin(char *)
            {
               cout << "CTextWin(char *tx)建構元被呼叫了..." << endl; 
               strcpy(text,tx);
            }      
            void show_text()
            {
               cout << "text = " << text << endl;
            }
      };

      int main(void)
      {
         CTextWin tx1("Hello C++"); 	        // 呼叫39~43行的CTextWin()建構元
         CTextWin tx2(60,70);		            // 呼叫34~38行的CTextWin()建構元

         tx1.show_member();   
         tx1.show_text();

         tx2.show_member();
         tx2.show_text();   

         system("pause");
         return 0;
      }
    ```
* 子類別呼叫拷貝建構元時，會先呼叫父類別拷貝建構元
* 以下例子因為子類別沒有定義拷貝建構元，因此使用預設的拷貝建構元，所以修改 tx1 之後 tx2 也被修改，兩個 text 都指向同一個字串：
  * ```c
      #include <iostream>
      #include <cstdlib>
      using namespace std;
      class CWin                                  // 定義CWin類別，在此為父類別
      {
         protected:
           char id;

         public:
           CWin(char i='D'):id(i)   	            // 父類別的建構元
           {
              cout << "CWin()建構元被呼叫了..." << endl;
           }
           CWin(const CWin& win)    	            // 父類別的拷貝建構元
           {
              cout << "CWin()拷貝建構元被呼叫了..." << endl;
              id=win.id;
           }
           ~CWin()     				            // 父類別的解構元
           {
              cout << "CWin()解構元被呼叫了... " << endl;
              system("pause");
           }
      };

      class CTextWin : public CWin                // 定義CTextWin類別，繼承自CWin類別
      {
         private:                  	            // 子類別裡的私有成員
            char *text;

         public:
            CTextWin(char i,char *tx):CWin(i)     // 子類別的建構元
            {
               cout << "CTextWin()建構元被呼叫了..." << endl;
               text= new char[strlen(tx)+1];
               strcpy(text,tx);
            }
            ~CTextWin()
            {
               delete [] text;  			        // 釋放text指標所指向的記憶體
               cout << "CTextWin()解構元被呼叫了... " << endl;
               system("pause");
            }
            void show_member()       		        // 子類別的show_member()函數
            {
               cout << "Window " << id << ": ";
               cout << "text = " << text << endl;
            }
            void set_member(char i,char *tx)      // 子類別的set_member()函數
            {
               id=i;
               delete [] text;               	    // 將原先指向的記憶體釋放
          text= new char[strlen(tx)+1];	    // 重新配置記憶體
               strcpy(text,tx);
            }
      };
      int main(void)
      {
         CTextWin tx1('A',"Hello C++");	        // 建立子類別的物件tx1
         CTextWin tx2(tx1);				        // 以tx1建立子類別的物件tx2

         tx1.show_member();
         tx2.show_member();

         cout << "更改tx1物件的成員之後..." << endl;
         tx1.set_member('B',"Welcome  C++");

         tx1.show_member();
         tx2.show_member();

         system("pause");
         return 0;
      }
    ```
  * 解決方法是在定意子類別的拷貝建構元
  * ```c
      CTextWin(const CTextWin &tx):CWin(tx)	   // 子類別的拷貝建構元，並指定先呼叫上述定義的父拷貝建構元，如果沒有定義的話會呼叫沒有引數的拷貝建構元
      {
         cout << "CTextWin()拷貝建構元被呼叫了..." << endl;
         text= new char[strlen(tx.text)+1];
         strcpy(text,tx.text);
      }
    ```



<h1 id="010">虛擬函數 & 抽象類別</h1> 

* 虛擬函數
  * 因為編譯器會把`show_area()`跟父類別的`area()`連結在一起編譯，這種函數連結的方式稱為早期連結(early binding)，或靜態連結(static linkage)，使用 virtual 的話就會與呼叫他的函數進行動態連結(dynamic linkage)，或晚期連結，也就是執行時才決定是哪一個`area()`被呼叫，而非編譯時就配對，以下例子如果沒有使用 virtual 的話，會呼叫父類別的`show_area()`：
  * ```c
      #include <iostream>
      #include <cstdlib>
      using namespace std;
      class CWin                      // 定義CWin類別，在此為父類別
      {
         protected:
           char id;
           int width, height;
         public:
           CWin(char i='D',int w=10, int h=10)   // 父類別的建構元
           {
              id=i;
              width=w;
              height=h;   
           }
            void show_area()	        // 父類別的show_area()函數
            {
               cout << "Window " << id << ", area = " << area() << endl;
            }
            int area()    		    // 父類別的area()函數
            {
               return width*height;
            }
      };

      class CMiniWin : public CWin  	// 定義子類別CMiniWin
      {
         public:     
           CMiniWin(char i,int w,int h):CWin(i,w,h){}  // 子類別的建構元

           int area()    				// 子類別的area()函數
           {
              return (int)(0.8*width*height);  
           }
      };

      int main(void)
      {
         CWin win('A',70,80);			// 建立父類別物件win
         CMiniWin m_win('B',50,60);	// 建立子類別物件m_win

         win.show_area();		        // 以父類別物件win呼叫show_area()函數
         m_win.show_area();	  	    // 以子類別物件m_win呼叫show_area()函數

         system("pause");
         return 0;
      }
    ```
* 抽象類別
  * 泛虛擬函數：為包含明確定義的虛擬函數
  * 包含泛虛擬函數的類別為抽象類別
  * 抽象類別不能用來產生物件
  * ```c
      class CShape
      {
          public:
              virtual int area()=0; // 定義 area()，並設之為0代表是一個泛虛擬函數
              
              void show_area()
              {
                  cout << "area = " << area() << endl;
              }
      }
    ```
  * 範例：
  * ```c
      #include <iostream>
      #include <cstdlib>
      using namespace std;
      class CShape                        // 定義抽象類別CShape
      {
         public:
            virtual int area()=0;  	    // 定義area()為泛虛擬函數

            void show_area()     	        // 定義成員函數show_area()
            {
               cout << "area = " << area() << endl;
            }   
      };

      class CWin : public CShape  	    // 定義由CShape所衍生出的子類別CWin
      {
         protected:
           int width, height;

         public:
           CWin(int w=10, int h=10)	    // CWin()建構元
           {
              width=w;
              height=h;   
           }
           virtual int area()   
           {
              return width*height;
           }
           void show_area()       
           {
              cout << "CWin物件的面積 = " << area() << endl;
           }        
      };

      class CCirWin : public CShape       // 定義由CShape所衍生出的子類別CCirWin
      {
         protected:
           int radius;

         public:
           CCirWin(int r=10)   		   // CCirWin()建構元
           {
              radius=r;
           }
           virtual int area()
           {
              return (int)(3.14*radius*radius);
           }
           void show_area()       
           {
              cout << "CCirWin物件的面積 = " << area() << endl;
           }   
      };

      class CMiniWin : public CWin        // 定義由CWin所衍生出的子類別CMiniWin
      {
         public:     
           CMiniWin(int w,int h):CWin(w,h){}         // 子類別的建構元

           virtual int area()
           {
              return (int) (0.5*width*height);
           }
           void show_area()       
           {
              cout << "CMiniWin物件的面積 = " << area() << endl;
           }       
      };

      int main(void)
      {
         CWin win1(50,60);
         CCirWin win2(100);
         CMiniWin win3(50,60);

         win1.show_area();
         win2.show_area();
         win3.show_area();

         system("pause");
         return 0;
      }
    ```
* 虛擬解構元：
  * 如果使用指標指向父類別(Base class)來建立物件時，編譯器只知道指標的型態為 Base class(CShape)，而不知道其實真正指向的是物件類型(CWin)，因此編譯器就會假設指標指向的就是 Base class 所建立的物件，就只會執行 Base class 所定義的 function。
  * 如果子類別的解構元沒有定義成虛擬解構元的話，那麼當我們`delete ptr`的時候，他就只會呼叫父類別的解構元，
  * 如下列範例所示，：
  * ```c
      #include <iostream>
      #include <cstdlib>
      using namespace std;
      class CShape                           // 定義抽象類別CShape
      {
         public:
            virtual int area()=0;            // 定義area()為泛虛擬函數
            void show_area()
            { 
               cout << "area = " << area() << endl;
            }  
            ~CShape()   	                   // ~CShape() 解構元 
            {
               cout << "~CShape()解構元被呼叫了..." << endl;
               system("pause");
            }          
      };

      class CWin : public CShape             // 定義由CShape所衍生出的子類別CWin
      {
         protected:
           int width, height;

         public:
           CWin(int w=10, int h=10):width(w),height(h){} // CWin()建構元

           virtual int area() {return width*height; }

           void show_area() {
              cout << "CWin物件的面積 = " << area() << endl;
           } 
           ~CWin()  		                               // ~CWin() 解構元 
           {
              cout << "~CWin()解構元被呼叫了..." << endl;
              system("pause");
           }   
      };

      class CMiniWin : public CWin           // 定義由CWin所衍生出的子類別CMiniWin
      {
         public:     
           CMiniWin(int w,int h):CWin(w,h){} // CMiniWin()建構元

           virtual int area() {
              return (int) (0.5*width*height);
           }
           void show_area(){
              cout << "CMiniWin物件的面積 = " << area() << endl;
           }       
           ~CMiniWin()   	                   // ~CMiniWin() 解構元 
           {
              cout << "~CMiniWin()解構元被呼叫了..." << endl;
              system("pause");
           }    
      };

      int main(void)
      {
         CShape *ptr=new CWin(50,60);	
         ptr->show_area();
         cout << "銷毀CWin物件..." << endl;
         delete ptr;
         cout << endl;     

         ptr=new CMiniWin(50,50);
         ptr->show_area();
         cout << "銷毀CMiniWin物件..." << endl;
         delete ptr;   
         cout << endl;

         CMiniWin m_win(100,100); 
         m_win.show_area();

         system("pause");
         return 0;
      }

    ```
  * 改善方法為把父類別的解構元改成虛擬解構元就好了：
  * ```c
      class CShape                        // 定義抽象類別CShape
      {
         public:
            virtual int area()=0;  		// 定義area()為泛虛擬函數
            virtual void show_area()    	// 定義show_area()為虛擬函數
            { 
               cout << "area = " << area() <<endl;
            }  
            virtual ~CShape()             // 定義 ~CShape() 為虛擬解構元 
            {
               cout << "~CShape()解構元被呼叫了..." << endl;
               system("pause");
            } 
      };
    ```


 <h1 id="011">檔案處理</h1> 

* 標頭檔：`#include <fstream>`
* `ifstream`：建立可供讀取資料的檔案物件
* `ofstream`：建立可供寫入資料的檔案物件
* `fstream`：建立可供讀/寫資料的檔案物件
* `ifstream`、`ofstream`、`fstream`分別繼承自`istream`、`ostream`、`iostream`，而`istream`、`ostream`、`iostream`又都繼承自`ios` class，因此 `ios` 類別的函數可以用在其他所有子類別上。
* 語法：
  * `檔案類別名稱 檔案物件("檔案名稱", ios::開啟模式)`
  * 或是：`檔案物件.open("檔案名稱", ios::開啟模式)`
* 開啟模式：
  * `ios::app`：可附加於檔尾(append)
  * `ios::binary`：二進位檔
  * `ios::in`：讀
  * `ios::out`：寫
  * `ios::trunc`：如果開啟的檔案已存在，則先刪除他，再開啟檔案
* 文字檔：
  * ```c
      #include <fstream>   // 載入fstream標頭檔
      #include <iostream>
      #include <cstdlib>
      using namespace std;
      int main(void)
      {
         /* write */
         ofstream ofile("c:\\donkey.txt",ios::out);  // 建立ofile物件

         if(ofile.is_open())                         // 測試檔案是否被開啟
         {
            ofile << "我有一隻小毛驢" << endl;       // 將字串寫入檔案
            ofile << "我從來也不騎" << endl;         // 將字串寫入檔案       
            cout << "已將字串寫入檔案..." << endl; 
         }
         else 
            cout << "檔案開啟失敗..."  << endl; 

         ofile.close();                              // 關閉檔案
         
         
         /* append */
         ofstream afile("c:\\donkey.txt",ios::app);   // 建立afile物件
   
         if(afile.is_open())                          // 測試檔案是否被開啟
         {
            afile << "有一天我心血來潮騎著去趕集";    // 將字串寫入檔案

            cout << "已將字串附加到檔案了..." <<endl; 
         }
         else 
            cout << "檔案開啟失敗..."  << endl; 

         afile.close();                               // 關閉檔案


         /* read */
         char txt[40];    			// 建立字元陣列，用來接收字串
         ifstream ifile("c:\\donkey.txt",ios::in);

         while(!ifile.eof())		    // 判別是否讀到檔案的尾端
         {
            ifile >> txt;       		// 將檔案內容寫入字元陣列
            cout << txt << endl; 
         }

         ifile.close();			    // 關閉檔案


         system("pause");
         return 0;
      }
    ```
  * `fileobj.get(ch);`：從檔案中讀取一個字元，並且入到 ch當中
  * `fileobj.getline(str, N, '\n')`：從檔案內讀取最多N-1個字元，或是讀取直到遇到`\n`為止。
  * `fileobj.put(ch)`：將字元變數的值寫入檔案內
* 二進位檔：
  * 函數：
    * `write((char*)&var, sizeof(var));`，須將要寫入的變數的位址強制轉換為`char*`
    * `read((char*)&var, sizeof(var));`
  * 範例(亦可寫入物件、結構等變數)：
    * 寫：
    * ```c
        #include <fstream> 		// 載入fstream標頭檔
        #include <iostream>
        #include <cstdlib>
        #include <cmath> 		// 載入數學函數庫cmath
        using namespace std;
        int main(void)
        {
           double num;  
           ofstream ofile("c:\\binary.dat",ios::binary);    // 開啟可供寫入的二進位檔

           for(int i=1;i<=5;i++)
           {
              num=sqrt((double)i);  // 將i轉成double，再計算sqrt(i)
              ofile.write((char*)&num,sizeof(num));         // 將num寫入二進位檔
           }
           cout << "已將資料寫入二進位檔了..." << endl;

           ofile.close();       	// 關閉檔案

           system("pause");
           return 0;
        }
      ```
    * 讀：
    * ```c
        #include <fstream>   			                  // 載入fstream標頭檔
        #include <iostream>
        #include <cstdlib>
        using namespace std;
        int main(void)
        {
           ifstream ifile("c:\\binary.dat",ios::binary);  // 開啟二進位檔
           double num;

           for(int i=1;i<=5;i++)
           {
              ifile.read((char*) &num,sizeof(num));       // 從二進位檔中讀取資料
              cout << num << endl;   	                  // 印出讀取的內容
           }   
           cout << "二進位檔已被讀取了..." << endl; 

           ifile.close();       		                  // 關閉檔案  
           system("pause");
           return 0;
        }
      ```

 <h1 id="012">try & catch</h1> 


* 語法：
  * ```c
      try{
          if (條件敘述)
              throw 例外物件
      }catch(型態 變數名稱){
          /* do something when catch exception */
      }
  * ```
* 範例：
  * [常見例外](https://shengyu7697.github.io/cpp-exception/)
  * ```c
      #include <iostream>   
      #include <cstdlib>
      using namespace std;
      int main(void)
      {
         int array[10];

         try
         {
            for(int i=0;i<=10;i++)
            {
               if(i>9)  throw "Index out of bound";  // 拋出字串型態的例外
               if(i*i>60)
                  throw i;                           // 拋出整數型態的例外
               else
                  array[i]=i*i;
            }
         }
         catch(const char *str)                      // 可捕捉字串型態的例外
         {
            cout << "捕捉到" << str << "例外..." << endl;
         }    
         catch(int i)                                // 可捕捉整數型態的例外
         {
            cout << i << "的平方值超過60了" << endl;
         }       

         system("pause");
         return 0;
      }
    ```
* catch 所有 exception：
  * ```c
       catch(...)            // 可接收任何型態的例外
       {
          cout << "捕捉到例外了..." << endl;
       }    
    ```


<h1 id="013">template</h1> 

* 進階版的多載
* 函數樣板：
  * 語法：
    * ```c
        template <class 型態變數1, class 型態變數2,...>
        回傳型態 函數名稱(型態變數 引數1, 型態變數 引數2,...)
        {
            /* do something */
        }
      ```
    * 或者可以把第二行跟第一行合併：
    * ```c
        template <class 型態變數1, class 型態變數2,...>回傳型態 函數名稱(型態變數 引數1, 型態變數 引數2,...)
        {
            /* do something */
        }
      ```
  * 範例：
    * ```c
        #include <iostream>   
        #include <cstdlib>
        using namespace std;
        template <class T>	       // 定義函數樣板 
        T add(T a,T b)  		        // add()的傳回型態為T，傳入的兩個引數型態也是T
        {
           T sum=a+b; 		          // 設定變數sum的型態為T，其值等於a+b
           return sum;
        }

        int main(void)
        {
           cout << "add(3,4)=" << add<int>(3,4) << endl;   
           cout << "add(3.2,4.6)=" << add<double>(3.2,4.6) << endl;
           
           /*  
            * 也可以省略add 之後的<>括號
           cout << "add(3,4)=" << add(3,4) << endl;   
           cout << "add(3.2,4.6)=" << add(3.2,4.6) << endl;
           */

           system("pause");
           return 0;
        }
      ```
  * 多個不同型態的引數：
    * ```c
        #include <iostream>   
        #include <cstdlib>
        using namespace std;
        template <class T1, class T2>    	// 定義函數樣板
        double average(T1 a,T2 b) // 定義average()，可接收T1與T2型態的變數
        {
           cout << "sizeof(a)= " << sizeof(a) << ", ";
           cout << "sizeof(b)= " << sizeof(b) << endl;   
           return (double)(a+b)/2;   		// 傳回變數a,b的平均值
        }

        int main(void)
        {
           cout << "average(3,4.2)= " << average<int,double>(3,4.2) << endl;
           cout << "average(5.7,12)= " <<  average<double,int>(5.7,12)  << endl;

           /*  
            * 也可以省略add 之後的<>括號
           cout << "average(3,4.2)= " << average(3,4.2) << endl;
           cout << "average(5.7,12)= " <<  average(5.7,12)  << endl;
           */

           system("pause");
           return 0;
        }
      ```
* 類別樣板：
  * 語法：
    * ```c
        template <class 型態變數1, class 型態變數2,...>
        class 類別名稱
        {
            /* do something */
        }
      ```
    * 建立物件：
    * `類別名稱<型態1, 型態2...> 物件名稱`
  * 範例：
    * ```c
        template <class T>          	              // 定義類別樣板
        class CWin
        {
           protected:
              T width, height;         	              // 宣告資料成員

           public:
              CWin(T w,T h):width(w),height(h){};     // 建構元

              void show(void);         	              // show()函數的原型
        };
      ```
    * 定義成員函數：
    * ```c
        template <class T>          	              // 定義show()函數
        void CWin<T>::show()
        {
           cout << "width=" << width << ", ";
           cout << "height=" << height << endl;
        }
      ```
    * main() 函數：
    * ```c
        int main(void)
        {
           CWin <int> win1(50,60);                    // 建立win1物件
           CWin <double> win2(50.25,60.74);           // 建立win2物件

           cout << "win1 object: ";
           win1.show();
           cout << "win2 object: ";
           win2.show();

           system("pause");
           return 0;
        }
      ```
* 樣板特殊化：
  * 類別樣板特殊化：
    * 語法：
      * ```c
          template <> class 類別名稱 <欲特殊化的型態>    // 特殊化的類別樣板
          {
             欲特殊化的型態 width,height;  // 資料成員的型態必須和欲特殊化的型態相同
             public:
                 CWin(int w, int h):width(w),height(h){};
                 int area(void){ return 0; }
          };
        ```
      * 範例：
      * ```c
          #include <iostream>   
          #include <cstdlib>
          using namespace std;
          template <class T>              // 類別樣板
          class CWin
          {
             T width,height;
             public:
               CWin(T w,T h):width(w),height(h){};

               T area(void){ return width*height; }
          };

          template <> class CWin <int>    // 特殊化的類別樣板
          {
               int width,height;
             public:
               CWin(int w, int h):width(w),height(h){};

               int area(void){ return 0; }
          };

          int main(void)
          {
             CWin <int> win1(50,60);
             CWin <double> win2(12.3,45.8);  
             CWin <short> win3(12,45);     

             cout << "win1 object: ";
             cout << win1.area() << endl;

             cout << "win2 object: ";   
             cout << win2.area() << endl;

             cout << "win3 object: ";   
             cout << win3.area() << endl;

             system("pause");
             return 0;
          }
        ```
  * 成員函數樣板特殊化(上面只有 `area()` 不一樣，所以不需要特地件很多個 class)：
    * 範例：
    * ```c
        #include <iostream>   
        #include <cstdlib>
        using namespace std;
        template <class T>        // 類別樣板
        class CWin
        {
           T width,height;
           public:
             CWin(T w,T h):width(w),height(h){};

             T area(void)
             {
                return width*height;
             }
        };

        template <> int CWin<int>::area(void)   // 類別樣板內成員函數的特殊化
        {
           return 0;
        }  

        int main(void)
        {
           CWin <int> win1(50,60);
           CWin <double> win2(12.3,45.8);  

           cout << "win1 object: ";
           cout << win1.area() << endl;

           cout << "win2 object: ";   
           cout << win2.area() << endl;

           system("pause");
           return 0;
        }
      ```

<h1 id="013">名稱空間</h1> 


* 名稱空間裡的變數不會與其他名稱空間相同名字的變數衝突
* 語法：
  * ```c
      namespace 名稱空間
      {
          /* do something */
      }
    ```
* 宣告與定義：
  * ```c
      namespace name1
      {
          int value;
      }
    ```
* 使用：
  * `name::value;`
* 範例：
  * ```c
      #include <iostream>   
      #include <cstdlib>

      namespace name1          // 設定名稱空間name1
      {
         int var=5;            // 在名稱空間name1內宣告變數var
      }
      using namespace std;
      int main(void)
      {
         int var=10;           // 宣告區域變數var

         cout << "in name1, var= " << name1::var << endl;
         cout << "var= " << var << endl;

         system("pause");
         return 0;
      }
    ```
* using：
  * ```c
      #include <iostream>   
      #include <cstdlib>

      namespace name1    		                              // 設定名稱空間name1
      {
         int var=5;
      }

      using namespace name1;	                              // 設定此行以下的程式碼均使用name1名稱空間
      using namespace std; 	                              // 設定此行以下的程式碼也使用std名稱空間
      int main(void)
      {
         cout << "var= " << var << endl;                    // 印出name1名稱空間內var的值

         int var=10;     		                              // 設定區域變數var
         cout << "main()裡的變數 var= " << var << endl;     // 印出區域變數var的值

         cout << "name1::var= " << name1::var << endl;      // 印出name1內var的值

         system("pause");
         return 0;
      }
    ```



* using 多個 namespace：
  * ```c
      #include <iostream>   
      #include <cstdlib>

      namespace name1                        // 設定名稱空間name1
      {
         int var=5;
      }

      namespace name2                        // 設定名稱空間name2
      {
         int var=10;
      }
      using namespace std;
      int main(void)
      {
         {  
            using namespace name1;           // 使用名稱空間name1
            cout << "in namespace name1: ";
            cout << "var= " << var << endl;
         }

         {
            using namespace name2;           // 使用名稱空間name2
            cout << "in namespace name2: ";   
            cout << "var= " << var << endl;
         }   

         system("pause");
         return 0;
      }
    ```

* 名稱空間 std 在 `<iostream>` 中定義，根據 ANSI C++ 的標準，C++標準函式庫裡所包含的所有函數、類別、物件，全部都定義在 std 裡面。






<h1 id="0014">多個檔案(含標頭檔)</h1> 

* 範例：
  * main.cpp：
    * ```c
      #include <iostream>
      #include <cstdlib>
      #include "cwin.h"     		// 載入cwin.h標頭檔
      using namespace std;

      int main(void)
      {
         CWin win1('A',50,60); 
         win1.show();

         system("pause");
         return 0;
      }
      ```
  * show.cpp：
    * ```c
      #include "cwin.h"  			// 載入cwin.h標頭檔
      #include <iostream>
      using namespace std;

      void CWin::show(void) 		// 定義show()函數
      {                 
         cout << "Window " << id << ":" << endl; 
         cout << "Area = " << width*height << endl;  
      }
      ```
  * cwin.h：
    * ```c
        class CWin    				     // 定義視窗類別CWin
        {
           protected:
             char id;
             int width;   
             int height;
          public:
             CWin(char ch, int w, int h):id(ch),width(w),height(h){}
             void show(void);    		 // 成員函數area()的原型
        };
      ```

