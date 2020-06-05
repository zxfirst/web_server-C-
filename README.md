# web_server linux 学习
## 有关在http部分使用到的函数的总结
### 1、recv函数原型：int recv（SOCKET s，char *buf，int len，int flags）；
#### 参数一：指定接收端套接字描述符；
#### 参数二：指明一个缓冲区，该缓冲区用来存放recv函数接收到的数据；
#### 参数三：指明buf的长度；
#### 参数四 ：一般置为0。
#### 如果recv函数成功，返回值大于0，返回的是其实际copy的字节数；
#### 如果recv函数返回-1，而且errno == EAGAIN || errno == EWOULDBLOCK，表示套接字已标记为非阻塞，而且超时；
#### 如果recv函数返回0，表示链接中断。

### 2、strpbrk函数原型：char *strpbrk(const char *str1, const char *str2)；
#### 检索字符串 str1 中第一个匹配字符串 str2 中字符的字符，不包含空结束字符；
#### 返回对应字符在str1中的地址。

### 3、strcasecmp函数原型：int strcasecmp (const char *str1, const char *str2);；
#### 比较字符串str1和字符串str2，不区分大小写；
#### 若参数str1和str2字符串相同则返回0。str1大于str1则返回大于0的值，str1若小于str1则返回小于0的值。

### 4、strncasecmp函数原型：int strcasecmp (const char *str1, const char *str2，int n);
#### 用来比较参数str1和str2字符串前n个字符，比较时会自动忽略大小写的差异;
#### 前n个字符中，若参数str1和str2字符串相同则返回0。str1大于str1则返回大于0的值，str1若小于str1则返回小于0的值。

### 5、strspn函数原型：size_t strspn(const char *str1, const char *str2)； 
#### 检索字符串str1中第一个不在字符串str2中出现的字符下标；
#### 返回str1中第一个不在字符串str2中出现的字符下标。

### 6、strcpy函数原型：char *strcpy(char *dest, const char *src)；
#### 将src所指向的字符串复制到 dest；
#### 需要注意的是如果目标数组dest不够大，而源字符串的长度又太长，可能会造成缓冲溢出的情况；
#### 返回个指向最终的目标字符串dest的指针。

### 7、strncpy函数原型：char *strncpy(char *dest, const char *src, size_t n)；
#### 把 src所指向的字符串复制到 dest，最多复制 n个字符。当src的长度小于n时，dest的剩余部分将用空字节填充；
#### 返回个指向最终的目标字符串dest的指针。

### 8、strlen函数原型：size_t strlen(const char* str);
#### 函数从字符串的开头位置依次向后计数，直到遇见\0，然后返回计时器的值，最终统计的字符串长度不包括\0；
#### 返回str指向的字符串的长度。

### 9、stat函数原型：int stat(const char *file_name, struct stat *buf);
#### 通过文件名filename获取文件信息，并保存在buf所指的结构体stat中；
#### 执行成功则返回0，失败返回-1，错误代码存于errno
#### errno：
##### ENOENT         参数file_name指定的文件不存在
##### ENOTDIR        路径中的目录存在但却非真正的目录
##### ELOOP          欲打开的文件有过多符号连接问题，上限为16符号连接
##### EFAULT         参数buf为无效指针，指向无法存在的内存空间
##### EACCESS        存取文件时被拒绝
##### ENOMEM         核心内存不足
##### ENAMETOOLONG   参数file_name的路径名称太长

#### struct stat {
####  dev_t         st_dev;       //文件的设备编号
####  ino_t         st_ino;       //节点
####  mode_t        st_mode;      //文件的类型和存取的权限
####  nlink_t       st_nlink;     //连到该文件的硬连接数目，刚建立的文件值为1
####  uid_t         st_uid;       //用户ID
####  gid_t         st_gid;       //组ID
####  dev_t         st_rdev;      //(设备类型)若此文件为设备文件，则为其设备编号
####  off_t         st_size;      //文件字节数(文件大小)
####  unsigned long st_blksize;   //块大小(文件系统的I/O 缓冲区大小)
####  unsigned long st_blocks;    //块数
####  time_t        st_atime;     //最后一次访问时间
####  time_t        st_mtime;     //最后一次修改时间
####  time_t        st_ctime;     //最后一次改变时间(指属性)
####  };

#### st_mode：
##### S_IFMT   0170000    文件类型的位遮罩
##### S_IFSOCK 0140000    scoket
##### S_IFLNK 0120000     符号连接
##### S_IFREG 0100000     一般文件
##### S_IFBLK 0060000     区块装置
##### S_IFDIR 0040000     目录
##### S_IFCHR 0020000     字符装置
##### S_IFIFO 0010000     先进先出

##### S_ISUID 04000     文件的(set user-id on execution)位
##### S_ISGID 02000     文件的(set group-id on execution)位
##### S_ISVTX 01000     文件的sticky位

##### S_IRUSR(S_IREAD) 00400     文件所有者具可读取权限
##### S_IWUSR(S_IWRITE)00200     文件所有者具可写入权限
##### S_IXUSR(S_IEXEC) 00100     文件所有者具可执行权限

##### S_IRGRP 00040             用户组具可读取权限
##### S_IWGRP 00020             用户组具可写入权限
##### S_IXGRP 00010             用户组具可执行权限

##### S_IROTH 00004             其他用户具可读取权限
##### S_IWOTH 00002             其他用户具可写入权限
##### S_IXOTH 00001             其他用户具可执行权限

##### S_ISLNK (st_mode)    判断是否为符号连接
##### S_ISREG (st_mode)    是否为一般文件
##### S_ISDIR (st_mode)    是否为目录
##### S_ISCHR (st_mode)    是否为字符装置文件
##### S_ISBLK (st_mode)        是否为先进先出
##### S_ISSOCK (st_mode)   是否为socket

