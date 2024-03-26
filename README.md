| 这个作业属于哪个课程 | <[软件工程2024 (广东工业大学)](https://edu.cnblogs.com/campus/gdgy/SoftwareEngineering2024)> |
| ----------------- |--------------- |
| 这个作业要求在哪里| <[结对项目](https://edu.cnblogs.com/campus/gdgy/SoftwareEngineering2024/homework/13137)> |
| 这个作业的目标 | <**团队互相合作完成一个随机生成四则运算题目的项目**> |
- Gitee链接：https://github.com/messagelost/Arithmetic-Operations
# <span style="color:black">一、团队介绍</span>
|项目合作者|学号|
| --------------------- | ----------------- |
|陈镜涵|3122004775         |
|阳昊|3122007304|
# <span style="color:black">二、PHP表格</span>
|PSP2.1|Personal Software Process Stages|预估耗时（分钟）|实际耗时（分钟）|
| ----------------- |----------- |--------------- |--------------- |
Planning|计划|20|35	
· Estimate|· 估计这个任务需要多少时间|20|35
Development|开发|1200|1400
· Analysis|· 需求分析 (包括学习新技术)|430|500
· Design Spec|· 生成设计文档|60|60
· Design Review|· 设计复审 |60|60
· Coding Standard|· 代码规范 (为目前的开发制定合适的规范)|30|30
· Design|· 具体设计|120|150
· Coding|· 具体编码|500|600
· Code Review|· 代码复审|60|60
· Test|· 测试（自我测试，修改代码，提交修改）|70|70
Reporting|报告	|120|150
· Test Repor|· 测试报告|30|40
· Size Measurement|· 计算工作量|30|40
· Postmortem & Process Improvement Plan|· 事后总结, 并提出过程改进计划|60|60
 ||合计|1340|1585
# <span style="color:black">三、效能分析</span>
- 对应的三个测试类
![](https://img2024.cnblogs.com/blog/2949708/202403/2949708-20240325230942809-823272633.png)
![](https://img2024.cnblogs.com/blog/2949708/202403/2949708-20240325230946328-1277471780.png)
![](https://img2024.cnblogs.com/blog/2949708/202403/2949708-20240325230959041-414344710.png)
![](https://img2024.cnblogs.com/blog/2949708/202403/2949708-20240326092245166-471124953.png)

# <span style="color:black">四、设计实现过程</span>
![](https://img2024.cnblogs.com/blog/2949708/202403/2949708-20240325215511220-1828801870.png)
- 1.**MathExerciseGenerator类：**：
    - **该类主要实现<span style="color:red">参数命令行程序、将生成的四则运算表达式和答案存入对应文件，生成答案正确错误的显示文件、对命令行的检索的功能</span>。**
![](https://img2024.cnblogs.com/blog/2949708/202403/2949708-20240325220308115-24167056.png)
- 2.**CreateOperation类：**：
    - **该类主要实现<span style="color:red">随机运算符、自然数、分数、自然数、括号的随机生成，还包括了生成有无括号的四则运算表达式的字符串</span>。**
- 3.**Fraction类：**：
    - **该类实现了<span style="color:red">真分数的结构以及运算函数</span>，有对真分数的加减乘除以及化简操作。**
- 4.**TransFraction类：**：
    - **该类实现了对随机数的转换，将随机生成的自然数，分数，真分数转化成包括><span style="color:red"> wholeNumber，numerator，denominato</span>的真分数。**
![](https://img2024.cnblogs.com/blog/2949708/202403/2949708-20240325221157540-340555678.png)
- 5.**ArithmeticOperation类：**：
    - **该类主要实现对有括号四则运算表达式中运算操作符的提取，并利用<span style="color:red">栈与Fraction类</span>计算出最终运算结果。**
- 6.**ArithmeticExpressionEquivalence类：**：
    - **该类主要利用<span style="color:red">逆波兰算法将中缀表达式转换为后缀表达式，用来观察各个式子运算顺序是否相同。</span>**
[参考链接](https://www.cnblogs.com/lanhaicode/p/10776166.html)
# <span style="color:black">五、代码说明</span>
- 核心代码
<details>
  <summary>CreateOperation</summary>
  
```public class CreateOperation {
    public static int nValue;//生成题目个数
    public static int rValue;//题目中数值范围
     public static void main(String[] args) {
        int n = nValue;
        int r = rValue;
        r =10;
        Random random = new Random();
         char []operater=Operator(r);
         //System.out.println(operater);
         int []decide=Decide(operater);
         //System.out.println(Arrays.toString(decide));
         int []calNumber=CalNumber(decide,r);
         //System.out.println(Arrays.toString(calNumber));
        char[]e = Create(decide,operater,calNumber);
        System.out.println(e);

    }

    public static char[] Create(int[] decide,char[]operater,int[]calNumber){
        Random random = new Random();
        int[]hesis=new int[1];
        hesis[0]= random.nextInt(2);//选择是否添加括号
        char[] e = My_string(decide,operater,calNumber).toCharArray();//数组e储存初始式子
        boolean CorreatOperation = true;
        while(true) {//判读式子是否正确
            for (int i = 0; i < e.length - 1; i++) {
                if (e[i] == '\u00F7' && e[i + 1] == '0') {//如果式子有除以0
                    CorreatOperation = false;
                }
            }
            if (CorreatOperation) {
                break;//式子合格跳出循环
            } else {
                CorreatOperation = true;
                e = My_string(decide,operater,calNumber).toCharArray();//再次生成式子
            }
        }
        if(operater.length!=1&&hesis[0]==1){
            e = Final_Expresion(String.valueOf(e),decide,operater,calNumber,hesis);
            //System.out.println(Arrays.toString(e1));
        }
        return e;
    }
    public static char[] Operator(int r) {
        Random random = new Random();
        int operatorNumber = random.nextInt(3) + 1;//随机生成1到3个运算符
        char[] operator = new char[operatorNumber];//数组存储运算符
        for (int i = 0; i < operatorNumber; i++) {//随机生成运算符
            int randomnumber = random.nextInt(4) + 1;
            switch (randomnumber) {
                case 1://生成加号
                    operator[i] = '+';
                    break;
                case 2://生成减号
                    operator[i] = '-';
                    break;
                case 3://生成乘号
                    operator[i] = '\u00D7';
                    break;
                case 4://生成除号
                    operator[i] = '\u00F7';
                    break;
            }
        }
        return operator;
    }
        public static int[]Decide(char [] operater){

        int operatorNumber=operater.length + 1;
        Random random = new Random();
        int calnumber = 0;
        int[] decide = new int[operatorNumber +1];//数组记录决定生成自然数（记为0）或真分数（记为1）带分数（记为2）
        for (int i = 0; i < operatorNumber; i++) {//决定生成自然数或者真分数
            int randomnumber = random.nextInt(3);
            if (randomnumber == 0) {
                decide[i] = 0;
                calnumber++;//自然数在数组中需要一位储存
            } else if (randomnumber == 1) {
                decide[i] = 1;
                calnumber = calnumber + 2;//真分数需要两位储存
            } else {
                decide[i] = 2;
                calnumber = calnumber + 3;//带分数在数组中需要三位储存
            }
        }
        decide[decide.length-1]=calnumber;
        return decide;
    }

       public static int[]CalNumber(int[]decide,int r) {
           Random random = new Random();
           int calnumber = decide[decide.length - 1];
           int[] calNumber = new int[calnumber];//数组储存要进行运算的数字
           int j = 0;
           for (int i = 0; i < calnumber; ) {//随机生成数字
               if (decide[j] == 0) {//生成自然数
                   int randomnumber = random.nextInt(r-1)+1;
                   calNumber[i] = randomnumber;
                   j++;
                   i++;
               } else if (decide[j] == 1) {//生成真分数
                   int[] faction = ProperFaction(r, 0);
                   for (int i1 = i; i1 < i + 2; i1++) {
                       calNumber[i1] = faction[i1 - i];
                   }
                   j++;
                   i = i + 2;
               } else {//生成带分数
                   int[] faction = ProperFaction(r, 1);
                   for (int i1 = i; i1 < i + 3; i1++) {
                       calNumber[i1] = faction[i1 - i];
                   }
                   j++;
                   i = i + 3;
               }
           }
           return calNumber;
       }
       public static String My_string(int[]decide,char []operator,int []calNumber){
           String fomula = new String();
           int count = 0;
           for (int i = 0; i < decide.length - 2; i++) {
               if (decide[i] == 0) {
                   fomula = fomula + String.valueOf(calNumber[count++]) + ' ' + operator[i] + ' ';
               } else if (decide[i] == 1) {
                   fomula = fomula + String.valueOf(calNumber[count]) + "/" + String.valueOf(calNumber[count + 1]) + ' ' + operator[i] + ' ';
                   count += 2;
               } else {
                   fomula = fomula + String.valueOf(calNumber[count]) + "'" + String.valueOf(calNumber[count + 1]) + "/" + String.valueOf(calNumber[count + 2]) + ' ' + operator[i] + ' ';
                   count += 3;
               }
           }
           //添加最后一个数值
           if (decide[decide.length - 2] == 0) {
               fomula = fomula + String.valueOf(calNumber[count++]);
           } else if (decide[decide.length - 2] == 1) {
               fomula = fomula + String.valueOf(calNumber[count]) + "/" + String.valueOf(calNumber[count + 1]);
               count += 2;
           } else {
               fomula = fomula + String.valueOf(calNumber[count]) + "'" + String.valueOf(calNumber[count + 1]) + "/" + String.valueOf(calNumber[count + 2]) ;
               count += 3;
           }
           //System.out.println(fomula);
        return fomula;
       }

    public  static char[] Final_Expresion(String fomula,int[]decide,char []operator,int []calNumber,int[] hesis){
         Random random = new Random();
        int chooseNumber1;//前括号的后一个数值
        int chooseNumber2;//后括号的前一个数值
        char[] operation = new char[0];
        int j = 0;
        int count = 0;
        while (true) {
            if (hesis[0] == 0 ) {
                operation = fomula.toCharArray();
                break;//hesis值为0则不添加括号
            }
            if(operator.length == 1){
                hesis[0] = 0;
                operation = fomula.toCharArray();
                break;
            }
            else {
                chooseNumber2 = random.nextInt(decide.length - 2) + 2;
                chooseNumber1 = random.nextInt(chooseNumber2 - 1) + 1;//随机选择
                //System.out.println(chooseNumber1);
                //System.out.println(chooseNumber2);

                if(chooseNumber1==1&&chooseNumber2== decide.length-1){
                    operation = fomula.toCharArray();
                    hesis[0]=0;
                    break;
                }else{
                    fomula = "";//清空字符串重新添加
                    for (int i = 0; i < decide.length - 2; i++) {
                        if(i==chooseNumber1-1){//添加前括号
                            if (decide[i] == 0) {
                                fomula = fomula + '(' + String.valueOf(calNumber[count++]) + ' ' + operator[i] + ' ';
                            } else if (decide[i] == 1) {
                                fomula = fomula + '(' + String.valueOf(calNumber[count]) + "/" + String.valueOf(calNumber[count + 1]) + ' ' + operator[i] + ' ';
                                count += 2;
                            } else {
                                fomula = fomula + '(' + String.valueOf(calNumber[count]) + "'" + String.valueOf(calNumber[count + 1]) + "/" + String.valueOf(calNumber[count + 2]) + ' ' + operator[i] + ' ';
                                count += 3;
                            }
                        } else if (i == chooseNumber2-1) {//添加后括号
                            if (decide[i] == 0) {
                                fomula = fomula + String.valueOf(calNumber[count++])+ ')' + ' ' + operator[i] + ' ' ;
                            } else if (decide[i] == 1) {
                                fomula = fomula + String.valueOf(calNumber[count]) + "/" + String.valueOf(calNumber[count + 1])+ ')' + ' ' + operator[i] + ' ' ;
                                count += 2;
                            } else {
                                fomula = fomula + String.valueOf(calNumber[count]) + "'" + String.valueOf(calNumber[count + 1]) + "/" + String.valueOf(calNumber[count + 2])+ ')' + ' ' + operator[i] + ' ' ;
                                count += 3;
                            }
                        }else{//添加数值
                            if (decide[i] == 0) {
                                fomula = fomula + String.valueOf(calNumber[count++]) + ' ' + operator[i] + ' ';
                            } else if (decide[i] == 1) {
                                fomula = fomula + String.valueOf(calNumber[count]) + "/" + String.valueOf(calNumber[count + 1]) + ' ' + operator[i] + ' ';
                                count += 2;
                            } else {
                                fomula = fomula + String.valueOf(calNumber[count]) + "'" + String.valueOf(calNumber[count + 1]) + "/" + String.valueOf(calNumber[count + 2]) + ' ' + operator[i] + ' ';
                                count += 3;
                            }
                        }
                    }
                    //添加最后一个数
                    if(chooseNumber2== decide.length-1){
                        if (decide[decide.length - 2] == 0) {
                            fomula = fomula + String.valueOf(calNumber[count++])+ ')';
                        } else if (decide[decide.length - 2] == 1) {
                            fomula = fomula + String.valueOf(calNumber[count]) + "/" + String.valueOf(calNumber[count + 1])+ ')';
                            count += 2;
                        } else {
                            fomula = fomula + String.valueOf(calNumber[count]) + "'" + String.valueOf(calNumber[count + 1]) + "/" + String.valueOf(calNumber[count + 2])+ ')';
                            count += 3;
                        }
                    }else{
                        if (decide[decide.length - 2] == 0) {
                            fomula = fomula + String.valueOf(calNumber[count++]);
                        } else if (decide[decide.length - 2] == 1) {
                            fomula = fomula + String.valueOf(calNumber[count]) + "/" + String.valueOf(calNumber[count + 1]);
                            count += 2;
                        } else {
                            fomula = fomula + String.valueOf(calNumber[count]) + "'" + String.valueOf(calNumber[count + 1]) + "/" + String.valueOf(calNumber[count + 2]) ;
                            count += 3;
                        }
                    }
                }
                operation = fomula.toCharArray();
                break;
            }
        }

        //System.out.println(operation);

        return operation;
    }
    public static int[] ProperFaction(int r,int t) {
        Random random = new Random();
        int[] faction = new int[2+t];
        if(t==1){//带分数
            int real=random.nextInt(r-1)+1;
            int denominator = random.nextInt(r-1)+1;//生成分母
            int numerator = random.nextInt(denominator);//生成分子
            faction[0] = real;
            faction[1] = numerator;
            faction[2] =  denominator;
        }else{//真分数
            int denominator = random.nextInt(r-1)+1;//生成分母
            int numerator = random.nextInt(denominator)+1;//生成分子
            faction[0] = numerator;
            faction[1] =  denominator;
        }
        return faction;
    }
    public static void Answer(String s){


    }
}
```
</details>

# <span style="color:black">六、测试运行</span>
- 一万道题目生成
![](https://img2024.cnblogs.com/blog/2949708/202403/2949708-20240326080659274-156211523.png)
![](https://img2024.cnblogs.com/blog/2949708/202403/2949708-20240326080703762-822057249.png)
![](https://img2024.cnblogs.com/blog/2949708/202403/2949708-20240326080706928-612495909.png)
- 答案校对
![](https://img2024.cnblogs.com/blog/2949708/202403/2949708-20240326094852154-1682055554.png)
![](https://img2024.cnblogs.com/blog/2949708/202403/2949708-20240326094856475-1581135497.png)
![](https://img2024.cnblogs.com/blog/2949708/202403/2949708-20240326094859653-727654999.png)
![](https://img2024.cnblogs.com/blog/2949708/202403/2949708-20240326103258138-433417716.png)


# <span style="color:black">七、项目小结</span>

- **总结：在过去的一段时间里，我们合作完成了一个四则运算表达式项目，旨在帮助学生练习基本的数学运算。通过这个项目，我们获得了一些宝贵的经验和教训，也取得了一些成就。**
**项目概述这个项目的目标是开发一个简单的四则运算表达式生成器，可以生成包含加减乘除的数学表达式，并验证用户输入的答案是否正确。我们的任务是设计算法、实现功能，并确保程序的正确性和稳定性。**

###成功之处
- 清晰的需求分析：我们在项目开始阶段进行了充分的需求分析，明确了项目的目标和功能要求，为后续的开发工作奠定了良好的基础。
- 有效的任务分工：我们根据各自的技术能力和兴趣，合理分配了任务，确保每个人都能充分发挥自己的优势，提高了工作效率。
- 良好的沟通与协作：我们保持定期的沟通，及时交流进展和遇到的问题，共同协作解决难题，确保项目顺利进行。
###需要改进之处
- 时间规划不足：在项目进行过程中，我们有时候会遇到一些时间紧迫的情况，导致工作压力较大。下次需要更合理地规划时间，避免出现时间不足的情况。
- 代码审查不够严格：有时候为了赶进度，我们可能会忽略一些代码细节上的问题，导致后期出现了一些bug。下次需要加强代码审查，确保代码质量。
###结对感受
- 在这次结对项目中，我们互相学习、互相支持，共同成长。通过合作，我们更加深入地了解了彼此的工作方式和思维模式，建立了良好的合作关系。
###闪光点与建议
- 对方的闪光点：我很欣赏对方在解决问题时的思维方式，总能想到一些我没有考虑到的方面，让我受益匪浅。
- 对方的建议：在工作中，对方提出了一些关于代码优化和算法改进的建议，让项目的效率得到了提高，下次我也会更加重视这些方面。
- 通过这次结对项目，我们不仅完成了任务，还建立了深厚的合作关系，相信在未来的工作中，我们能够继续携手并进，取得更大的成就。
