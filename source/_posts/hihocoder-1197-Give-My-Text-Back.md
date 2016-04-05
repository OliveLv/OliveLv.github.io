title: 'hihocoder 1197:Give My Text Back'
date: 2016-04-05 16:51:27
tags:
- hihocoder
---
## 1197 : Give My Text Back
时间限制:10000ms
单点时限:1000ms
内存限制:256MB
描述
To prepare for the English exam Little Ho collected many digital reading materials. Unfortunately the materials are messed up by a malware.

It is known that the original text contains only English letters (a-zA-Z), spaces, commas, periods and newlines, conforming to the following format:

1. Each sentence contains at least one word, begins with a letter and ends with a period.

2. In a sentence the only capitalized letter is the first letter.

3. In a sentence the words are separated by a single space or a comma and a space.

4. The sentences are separated by a single space or a single newline.

It is also known the malware changes the text in the following ways:

1. Changing the cases of letters.

2. Adding spaces between words and punctuations.

Given the messed text, can you help Little Ho restore the original text?

**输入**
>A string containing no more than 8192 English letters (a-zA-Z), spaces, commas, periods and newlines which is the messed text.

**输出**
>The original text.

**样例输入**
>my Name  is Little   Hi.
His   name IS Little ho  ,  We are   friends.    

**样例输出**
>My name is little hi.
His name is little ho, we are friends.



```java
/**   
 * @Title: GiveMyTextBack.java 
 * @author olivelv  
 * @date Apr 4, 2016 4:11:53 PM  
 * 
 */

/**
 * test:
 *  my Name  is Little   Hi. ami           .i hate      hiho                     ,codr. EEENNN.
   His   name IS Little ho  ,  We are   friends.
 my Name  is Little   Hi.His   name IS Little ho  ,  We are   friends.
my Name  is Little   Hi. His   name IS Little ho  ,  We are   friends.
 His   name IS Little ho  ,We are   friends.   weoifj   ,jo.
Jo, f, j l, o,o, l.
result:
My name is little hi. Ami. I hate hiho, codr. Eeennn.
His name is little ho, we are friends.
My name is little hi. His name is little ho, we are friends.
My name is little hi. His name is little ho, we are friends.
His name is little ho, we are friends. Weoifj, jo.
规则：
1、遇到','或者'.'如果后面还有内容，则要用“空格”间隔开
2、每个输入可能宝航多个句子  每个句子的开头单词的首字母都要大写
3、整段句的最后以'.'结尾，不应多加空格，否则会报“PE”
解题思路
首先将字符串转换为小写。使用标记量flag和first进行标识。
flag 记录前一个字符是否为空格  flag=0 表示前一个字符为空格，flag=1表示前一个为非空格
first 记录是否是句首 first=0表示句首，first=1 表示非句首
遇到','或'.'，则在其后添加“空格”。遇到字母时，若不是句首（first！=0），前一个字符为空格（flag=0），且结果字符最后一个字符不是空格，则添加一个“空格”。
 *
 */
public class GiveMyTextBack {
	public static void main(String []args){
		Scanner cin=new Scanner(System.in);
		String s;
		while(cin.hasNext()){
			s=cin.nextLine();
			s=s.toLowerCase();
			int n=s.length();
			int flag=0;
			int first=0;
			String res="";
			for(int i=0;i<n;i++){
				char tmp=s.charAt(i);
				if(tmp!=' '){
					if(tmp==','){
						res+=tmp;
						res+=' ';
					}
					else if(tmp=='.'){
						res+=tmp+" ";
						first=0;
					}
					else{
						if(flag==0&&first!=0&&!res.endsWith(" "))res+=' ';
						if(first==0){
							res+=(char)(tmp-'a'+'A');
							first=1;
						}else
							res+=tmp;
						flag=1;
					}
				}else
					flag=0;
			}
			System.out.println(res.trim());
		}
	}
}
```
