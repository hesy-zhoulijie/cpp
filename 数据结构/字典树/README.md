# 字典树

## 资源目录
[文件目录](https://gitee.com/zljzljsweepy/Github-files-sync/tree/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/%E5%AD%97%E5%85%B8%E6%A0%91)
![](https://pic.downk.cc/item/5f18f86d14195aa594e7c17d.jpg)

Trie 树，即字典树，又称单词查找树或键树，是一种树形结构，是一种哈希树的变种。典型应用是用于统计和排序大量的字符串（但不仅限于字符串），所以经常被搜索引擎系统用于文本词频统计。它的优点是：最大限度地减少无谓的字符串比较。

Trie 的核心思想是空间换时间。利用字符串的公共前缀来降低查询时间的开销以达到提高效率的目的。

## 前缀树的 3 个基本性质：

1.  根节点不包含字符，除根节点外每一个节点都只包含一个字符。
2.  从根节点到某一节点，路径上经过的字符连接起来，为该节点对应的字符串。
3.  每个节点的所有子节点包含的字符都不相同。

## 前缀树查询和哈希查询的比较（示相对情况而定）：

通常字典树的查询时间复杂度是 O(logL)，L 是字符串的长度。所以效率还是比较高的。

网上的一部分文章说的都是字典树的效率比 hash 表高。我觉得还是相对来看比较好，各有个的特点吧。

hash 表，通过 hash 函数把所有的单词分别 hash 成 key 值，查询的时候直接通过 hash 函数即可，都知道 hash 表的效率是非常高的为 O(1)，直接说字典树的查询效率比 hash 高，难道有比 O(1) 还快的 - -。

hash：

当然对于单词查询，如果我们 hash 函数选取的好，计算量少，且冲突少，那单词查询速度肯定是非常快的。那如果 hash 函数的计算量相对大呢，且冲突律高呢？

这些都是要考虑的因素。且 hash 表不支持动态查询，什么叫动态查询，当我们要查询单词 apple 时，hash 表必须等待用户把单词 apple 输入完毕才能 hash 查询。

当你输入到 appl 时肯定不可能 hash 吧。

字典树（tries 树）：

对于单词查询这种，还是用字典树比较好，但也是有前提的，空间大小允许，字典树的空间相比较 hash 还是比较浪费的，毕竟 hash 可以用 bit 数组。

那么在空间要求不那么严格的情况下，字典树的效率不一定比 hash 若，它支持动态查询，比如 apple，当用户输入到 appl 时，字典树此刻的查询位置可以就到达 l 这个位置，那么我在输入 e 时光查询 e 就可以了（更何况如果我们直接用字母的 ASCII 作下标肯定会更快）！字典树它并不用等待你完全输入完毕后才查询。

所以效率来讲我认为是相对的。

---------------------------------------------------------------------------------------------------------------------------

接下来通过案例来介绍前缀树的具体操作。

题目：给你 100000 个长度不超过 10 的单词。对于每一个单词，我们要判断他出没出现过，如果出现了，求第一次出现在第几个位置。

如果我们使用一般的方法，没查询一个单词都去遍历一遍，那么时间复杂度将为 O(n^2), 这对于 100000 这么大的数据是不能够接受的。假如我们要查找单词 student。那我们通过前缀树只需要查找 s 开头的即可，然后接下来查询 t 开头的即可，对于大量的数据可以省去不小的时间。

## 树结构：

其中 count 表示以当前单词结尾的单词数量。

prefix 表示以该处节点之前的字符串为前缀的单词数量。

```cpp
public class TrieNode {
	int count;
	int prefix;
	TrieNode[] nextNode=new TrieNode[26];
	public TrieNode(){
		count=0;
		prefix=0;
	}
}
```

### 1. 前缀树的创建

好比假设有 b，abc，abd，bcd，abcd，efg，hii 这 6 个单词, 那我们创建 trie 树就得到

![](https://pic3.zhimg.com/v2-9d07fbd164fc0d737aabe428b4484bd1_b.png)![](https://pic3.zhimg.com/80/v2-9d07fbd164fc0d737aabe428b4484bd1_720w.png)

```cpp
//插入一个新单词
	public static void insert(TrieNode root,String str){
		if(root==null||str.length()==0){
			return;
		}
		char[] c=str.toCharArray();
		for(int i=0;i<str.length();i++){
			//如果该分支不存在，创建一个新节点
			if(root.nextNode[c[i]-'a']==null){
				root.nextNode[c[i]-'a']=new TrieNode();
			}
			root=root.nextNode[c[i]-'a'];
			root.prefix++;//注意，应该加在后面
			}
		
		//以该节点结尾的单词数+1
		root.count++;
	}
```

### 2. 查询以 str 开头的单词数量，查询单词 str 的数量

```cpp
//查找该单词是否存在，如果存在返回数量，不存在返回-1
	public static int search(TrieNode root,String str){
		if(root==null||str.length()==0){
			return -1;
		}
		char[] c=str.toCharArray();
		for(int i=0;i<str.length();i++){
			//如果该分支不存在，表名该单词不存在
			if(root.nextNode[c[i]-'a']==null){
				return -1;
			}
			//如果存在，则继续向下遍历
			root=root.nextNode[c[i]-'a'];	
		}
		
		//如果count==0,也说明该单词不存在
		if(root.count==0){
			return -1;
		}
		return root.count;
	}
	
	//查询以str为前缀的单词数量
	public static int searchPrefix(TrieNode root,String str){
		if(root==null||str.length()==0){
			return -1;
		}
		char[] c=str.toCharArray();
		for(int i=0;i<str.length();i++){
			//如果该分支不存在，表名该单词不存在
			if(root.nextNode[c[i]-'a']==null){
				return -1;
			}
			//如果存在，则继续向下遍历
			root=root.nextNode[c[i]-'a'];	
		}
		return root.prefix;
	}
```

### **3. **在主函数中测试****

```cpp
public static void main(String[] args){
		TrieNode newNode=new TrieNode();
		insert(newNode,"hello");
		insert(newNode,"hello");
		insert(newNode,"hello");
		insert(newNode,"helloworld");
		System.out.println(search(newNode,"hello"));
		System.out.println(searchPrefix(newNode,"he"));
	}

/**
输出：3   4
**/
```

