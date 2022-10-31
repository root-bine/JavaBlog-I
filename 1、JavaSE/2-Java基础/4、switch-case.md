# switch-case

- 格式：

  ```java 
  switch(...){
  
  	case ... :
  
  		......;
  
  		break;
      ...
      default :
          ...
  }
  ```

  

- switch() 括号内兼容

  数据类型：byte、short、int 、char

  引用数据类型：String、eume
  
- 代码

  ```java
  public class demo01 {
      public static void main(String []args){
          //输入一个值
          Scanner scanner = new Scanner(System.in);
          int a = scanner.nextInt();
          switch(a){
              case 1:
                  System.out.println("A");
                  break;
              case 2:
                  System.out.println("B");
                  break;
              case 3:
                  System.out.println("C");
                  break;
              default :
                  System.out.println("无结果");
          }
      }
  }
  ```

  