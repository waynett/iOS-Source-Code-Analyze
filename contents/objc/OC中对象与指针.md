#                           关于OC中对象与指针的思考                                                                                                                                                            

1. 如下通过*定义的变量person，是一个指针变量，指针变量存储的是一个地址，person的值是一个地址，

2. person变量存在于当前线程的函数栈中，与argc、argv地址在一个栈空间。

3. person指针变量存储的是一个地址，该地址即指向我们的OC对象，即指向Person的实例对象。

4. OC的实例对象被分配在heap堆中。

5. 通过属性的setXXX，我们获取了OC实例对象的属性地址，该地址和OC实例对象一样都在heap中

   ```objective-c
   #import <Foundation/Foundation.h>
   
   int * pCount;
   
   @interface Person : NSObject
   
   @property(nonatomic,assign) int count;
   
   @end
   
   @implementation Person
   
   - (void)setCount:(int)count;
   {
       pCount = &_count;
       _count = count;
   }
   
   - (int)getCount;
   {
       return _count;
   }
   
   
   @end
   
   
   int main(int argc, const char * argv[]) {
       @autoreleasepool {
           
           Person * person = [[Person alloc] init];
           
           person.count = 1;
           
           NSLog(@"argc address=%p,argv address=%p, person self address in stack =%p,person value(equal to Person instancce addresss)=%@,person.count address=%p(Person instancce ivar address)",&argc,&argv,&person,person,pCount);
           
       }
       return 0;
   }
   
   ```

   ![09A119F9-AE39-42C6-ACED-2D6BBC23886C](/Users/waynettMini/svn/iOS-Source-Code-Analyze/contents/objc/OC中对象与指针.assets/09A119F9-AE39-42C6-ACED-2D6BBC23886C.png)

![545B605E-7065-49EB-AA07-CA087BC7FEDA](/Users/waynettMini/svn/iOS-Source-Code-Analyze/contents/objc/OC中对象与指针.assets/545B605E-7065-49EB-AA07-CA087BC7FEDA.png)

