
#### 1、给你四个坐标点，判断它们能不能组成一个矩形，如判断([0,0],[0,1],[1,1],[1,0])能组成一个矩形。
>
> 解题思路：
>
> 以一个点为参考点。分别计算出到任意三点的距离，最长的距离的一个一个点是对角线的点。
>
> 剩下的两个点是左右两个点。 用剩下的两个点计算出另外一个对角线的距离。如果两条对角线相等，
>
> 判断任意一个角是不是直角。即可判断是不是矩形了。

- [php实现](https://github.com/xianyunyh/arithmetic-php/blob/master/package/Other/Square.php)

#### 2、写一段代码判断单向链表中有没有形成环，如果形成环，请找出环的入口处，即P点 
```php
/*
 *单链表的结点类
 */
class LNode{
    //为了简化访问单链表,结点中的数据项的访问权限都设为public
    public int data;
    public LNode next;
}


class LinkListUtli {
    //当单链表中没有环时返回null，有环时返回环的入口结点
    public static LNode searchEntranceNode(LNode L)
    {
        LNode slow=L;//p表示从头结点开始每次往后走一步的指针
        LNode fast=L;//q表示从头结点开始每次往后走两步的指针
        while(fast !=null && fast.next !=null) 
        {
            if(slow==fast) break;//p与q相等，单链表有环
            slow=slow.next;
            fast=fast.next.next;
        }
        if(fast==null || fast.next==null) return null;

        // 重新遍历，寻找环的入口点
        slow=L;
        while(slow!=fast)
        {
            slow=slow.next;
            fast=fast.next;
        }

        return slow;
    }
}
```

#### 3、写一个函数，获取一篇文章内容中的全部图片，并下载
```php
function download_images($article_url = '', $image_path = 'tmp'){

    // 获取文章类容
    $content = file_get_contents($article_url);

    // 利用正则表达式得到图片链接
    $reg_tag = '/<img.*?\"([^\"]*(jpg|bmp|jpeg|gif|png)).*?>/';
    $ret = preg_match_all($reg_tag, $content, $match_result); 
    $pic_url_array = array_unique($match_result1[1]);

    // 创建路径
    $dir = getcwd() . DIRECTORY_SEPARATOR .$image_path;
    mkdir(iconv("UTF-8", "GBK", $dir), 0777, true);
    

    foreach($pic_url_array as $pic_url){
        // 获取文件信息
        $ch = curl_init($pic_url);
        curl_setopt($ch, CURLOPT_HEADER, 0);
        curl_setopt($ch, CURLOPT_NOBODY, 0);
        curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE );
        curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, FALSE );
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
        $fileInfo = curl_exec($ch);
        $httpinfo = curl_getinfo($ch);
        curl_close($ch);

        // 获取图片文件后缀
        $ext = strrchr($pic_url, '.');
        $filename = $dir . '/' . uniqid() . $ext; 

        // 保存图片信息到文件
        $local_file = fopen($filename, 'w');
        if(false !== $local_file){
            if( false !== fwrite($local_file, $filecontent) ){
            fclose($local_file);
            }
        }
    }

}
```


#### 4、从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是连续的

- [思路解析](https://blog.csdn.net/Jarvan_Song/article/details/52416039)
- [PHP代码实现](https://github.com/xianyunyh/arithmetic-php/blob/master/package/Other/Judge.php)

#### 5、两条相交的单向链表，如何求它们的第一个公共节点
思路：

1. 如果两个链表相交，则从相交点开始，后面的节点都相同，即**最后一个节点肯定相同；**


2. 从头到尾遍历两个链表，并记录链表长度，当二者的尾节点不同，则二者肯定不相交；


3. 尾节点相同，如果A长为LA，B为LB，如果LA>LB,则A前LA-LB个先跳过，

```php
class Node {
  public $data = NULL;
  public $next;
}

function getNodeLength(Node $node) {
  if(empty($node)) return 0;
  $length  = 0;
  while($node != NULL ) {
    $lenght++；
  }
  return $length;
}
/**
 * 求两个单链表相交的第一个公共节点
 */
function getPoint(Node $a,Node $b) {
  $a_length =getNodeLength($a);
  $b_length = getNodeLenght($b);
  //交换两个链表
  if($a_lenght < $b_lenght) {
    $temp = $a;
    $a = $b;
    $b = $temp;
  }
  $step = abs($a_length - $b_length);
  //先让a走step
  for($i = 0; $i<$step;$i++) {
    $a = $a->next;
  }
  //同步走
  while($a && $a !== $b) {
    $a = $a->next;
    $b = $b->next;
  }
  return $a;
  
}
```


#### 6、最长公共子序列问题LCS，如有[1,2,5,11,32,15,77]和[99,32,15,5,1,77]两个数组，找到它们共同都拥有的数，写出时间复杂度最优的代码，不能用array_intersect（这里有坑，需要去研究一下动态规划）。 

#### 7、获取当前客户端的IP地址，并判断是否在（111.111.111.111,222.222.222.222)**

注意:（111.111.111.111,222.222.222.222) 这是一个集合区间，不是数组的array

利用php获取ip地址。 然后转成long

```php
public function ip() {
    //strcasecmp 比较两个字符，不区分大小写。返回0，>0，<0。
    if(getenv('HTTP_CLIENT_IP') && strcasecmp(getenv('HTTP_CLIENT_IP'), 'unknown')) {
        $ip = getenv('HTTP_CLIENT_IP');
    } elseif(getenv('HTTP_X_FORWARDED_FOR') && strcasecmp(getenv('HTTP_X_FORWARDED_FOR'), 'unknown')) {
        $ip = getenv('HTTP_X_FORWARDED_FOR');
    } elseif(getenv('REMOTE_ADDR') && strcasecmp(getenv('REMOTE_ADDR'), 'unknown')) {
        $ip = getenv('REMOTE_ADDR');
    } elseif(isset($_SERVER['REMOTE_ADDR']) && $_SERVER['REMOTE_ADDR'] && strcasecmp($_SERVER['REMOTE_ADDR'], 'unknown')) {
        $ip = $_SERVER['REMOTE_ADDR'];
    }
    $res =  preg_match ( '/[\d\.]{7,15}/', $ip, $matches ) ? $matches [0] : '';
    return $ip;
}
$ip = sprintf('%u',ip2long(ip());
$begin = sprintf('%u',ip2long('111.111.111.111'))
$end = sprintf('%u',ip2long('222.222.222.222'))
if($ip > $begin && $ip < $end) {
    echo "在这个区间里"。
}
```