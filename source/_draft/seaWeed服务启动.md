# [SeaweedFS](https://github.com/chrislusf/seaweedfs)

* [SeaWeedFS-下载](https://github.com/chrislusf/seaweedfs/releases)    
* [SeaWeedFS - github主页](ttps://github.com/chrislusf/seaweedfs)    

## 操作环境说明
* weed 版本： 0.77
* 操作系统： macOS High Sierra 10.13.4

## 1. 启动服务
### * 启动 master 服务
```
weed master
```
> 启动后默认监听端口如下：    
>     master server : localhost:9333    
>     http listen : localhost:8080

### * 启动卷服务：    
```shell
weed volume -dir="/Users/hanlyjiang/Downloads/temp/weed/data" -max=5  -mserver="localhost:9333" -port=8080 
```

### * 启动 `filer` 服务    
此服务非必须启动    
```
weed filer --port 8889
```

> **注意：**    
> 到这里已经有好几个端口了，下面的操作需要注意请求对应的端口        
> * master - 9333 
> * volume - 8080 
> * filer - 8889     
> **可以试着在浏览器中打开**:
> * http://localhost:9333, 
> * http://localhost:8080, 
> * http://localhost:8889


## 2. 使用    

### 文件上传 

#### 1. 申请id上传，然后上传（9333 -> 8080）

```
> curl -X POST http://localhost:9333/dir/assign
{"count":1,"fid":"3,01637037d6","url":"127.0.0.1:8080","publicUrl":"localhost:8080"}

> curl -X PUT -F file=@/home/chris/myphoto.jpg http://127.0.0.1:8080/3,01637037d6
{"size": 43234}

```

#### 2. master 快捷上传（9333）    
```
curl -F file=@/root/seaweedfs-test/imgs/01.jpg http://localhost:9333/submit
```

#### 3. 使用 filer 上传 (8889)    
```
# 上传到指定目录
curl -F file=@github-垃圾.png http://localhost:8889/images/
# 上传重命名
curl -F file=@report.js "http://localhost:8889/javascript/new_report.js"

```


### 获取文件夹中文件列表
```

```


## 附： curl 命令简单说明
> curl 是用于传输数据到服务器的命令行工具, 支持协议： DICT, FILE, FTP, FTPS, GOPHER,HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMB, SMBS, SMTP, SMTPS, TELNET and TFTP。    
> curl 提供了一些有用的工具，如proxy支持,用户认证(authentication), FTP上传，HTTP post, SSL connections, cook-ies, file transfer resume, Metalink 等等.     

上面相关的curl命令option解释：    
* `-X, --request <command>` ： 在HTTP请求中用于指定请求方法
*  `-F, --form <name=content>` ： 模拟表单提交，`Content-Type` 会被设置为 `multipart/form-data` 



----------
部分命令选项摘录（man curl）：    
```
-F, --form <name=content>
    (HTTP) This lets curl emulate a filled-in form in which a user has pressed the submit button. This causes curl to  POST
              data  using  the Content-Type multipart/form-data according to RFC 2388. This enables uploading of binary files etc. To
              force the 'content' part to be a file, prefix the file name with an @ sign. To just get the content part from  a  file,
              prefix  the file name with the symbol <. The difference between @ and < is then that @ makes a file get attached in the
              post as a file upload, while the < makes a text field and just get the contents for that text field from a file.

              Example: to send an image to a server, where 'profile' is the name of the form-field to which portrait.jpg will be  the
              input:

               curl -F profile=@portrait.jpg https://example.com/upload.cgi

              To  read  content  from stdin instead of a file, use - as the filename. This goes for both @ and < constructs. Unfortu-
              nately it does not support reading the file from a named pipe or similar, as it needs the full size before the transfer
              starts.

--connect-timeout <seconds>

-b, --cookie <data>
        (HTTP) Pass the data to the HTTP server in the Cookie header. It is supposedly the data previously  received  from  the
        server in a "Set-Cookie:" line.  The data should be in the format "NAME1=VALUE1; NAME2=VALUE2".

--digest
              (HTTP)  Enables HTTP Digest authentication. This is an authentication scheme that prevents the password from being sent
              over the wire in clear text. Use this in combination with the normal -u, --user option to set user name and password.

              If this option is used several times, only the first one is used.

              See also -u, --user and --proxy-digest and --anyauth. This option overrides --basic and --ntlm and --negotiate.

--data-ascii <data>
              (HTTP) This is just an alias for -d, --data.

--data-binary <data>
              (HTTP) This posts data exactly as specified with no extra processing whatsoever.

              If you start the data with the letter @, the rest should be a filename.  Data is posted in  a  similar  manner  as  -d,
              --data does, except that newlines and carriage returns are preserved and conversions are never done.

              If this option is used several times, the ones following the first will append data as described in -d, --data.

--data-raw <data>
              (HTTP) This posts data similarly to -d, --data but without the special interpretation of the @ character.

              See also -d, --data. Added in 7.43.0.

--data-urlencode <data>
              (HTTP) This posts data, similar to the other -d, --data options with the exception that this performs URL-encoding.

              To  be CGI-compliant, the <data> part should begin with a name followed by a separator and a content specification. The
              <data> part can be passed to curl using one of the following syntaxes:

              content
                     This will make curl URL-encode the content and pass that on. Just be careful so that the content doesn't contain
                     any = or @ symbols, as that will then make the syntax match one of the other cases below!

              =content
                     This will make curl URL-encode the content and pass that on. The preceding = symbol is not included in the data.

              name=content
                     This will make curl URL-encode the content part and pass that on. Note that the name part is expected to be URL-
                     encoded already.

              @filename
                     This  will make curl load data from the given file (including any newlines), URL-encode that data and pass it on
                     in the POST.

              name@filename
                     This will make curl load data from the given file (including any newlines), URL-encode that data and pass it  on
                     in the POST. The name part gets an equal sign appended, resulting in name=urlencoded-file-content. Note that the
                     name is expected to be URL-encoded already.

       See also -d, --data and --data-raw. Added in 7.18.0.

       -d, --data <data>
              (HTTP) Sends the specified data in a POST request to the HTTP server, in the same way that a browser does when  a  user
              has filled in an HTML form and presses the submit button. This will cause curl to pass the data to the server using the
              content-type application/x-www-form-urlencoded.  Compare to -F, --form.

              --data-raw is almost the same but does not have a special interpretation of  the  @  character.  To  post  data  purely
              binary,  you  should instead use the --data-binary option.  To URL-encode the value of a form field you may use --data-
              urlencode.

              If any of these options is used more than once on the same command line, the  data  pieces  specified  will  be  merged
              together with a separating &-symbol. Thus, using '-d name=daniel -d skill=lousy' would generate a post chunk that looks
              like 'name=daniel&skill=lousy'.

              If you start the data with the letter @, the rest should be a file name to read the data from, or - if you want curl to
              read  the  data from stdin. Multiple files can also be specified. Posting data from a file named from a file like that,
              carriage returns and newlines will be stripped out. If you don't want the @ character to have a special  interpretation
              use --data-raw instead.

              See  also  --data-binary  and  --data-urlencode  and  --data-raw.  This  option overrides -F, --form and -I, --head and
              --upload.
                     name is expected to be URL-encoded already.

       See also -d, --data and --data-raw. Added in 7.18.0.

       -d, --data <data>
              (HTTP) Sends the specified data in a POST request to the HTTP server, in the same way that a browser does when  a  user
              has filled in an HTML form and presses the submit button. This will cause curl to pass the data to the server using the
              content-type application/x-www-form-urlencoded.  Compare to -F, --form.

                     This will make curl load data from the given file (including any newlines), URL-encode that data and pass it  on
                     in the POST. The name part gets an equal sign appended, resulting in name=urlencoded-file-content. Note that the
                     name is expected to be URL-encoded already.

       See also -d, --data and --data-raw. Added in 7.18.0.

       -d, --data <data>
              (HTTP) Sends the specified data in a POST request to the HTTP server, in the same way that a browser does when  a  user
              has filled in an HTML form and presses the submit button. This will cause curl to pass the data to the server using the
              content-type application/x-www-form-urlencoded.  Compare to -F, --form.

              --data-raw is almost the same but does not have a special interpretation of  the  @  character.  To  post  data  purely
              binary,  you  should instead use the --data-binary option.  To URL-encode the value of a form field you may use --data-
              urlencode.

              If any of these options is used more than once on the same command line, the  data  pieces  specified  will  be  merged
              together with a separating &-symbol. Thus, using '-d name=daniel -d skill=lousy' would generate a post chunk that looks
              like 'name=daniel&skill=lousy'.

              If you start the data with the letter @, the rest should be a file name to read the data from, or - if you want curl to
              read  the  data from stdin. Multiple files can also be specified. Posting data from a file named from a file like that,
              carriage returns and newlines will be stripped out. If you don't want the @ character to have a special  interpretation
              use --data-raw instead.

              See  also  --data-binary  and  --data-urlencode  and  --data-raw.  This  option overrides -F, --form and -I, --head and
              --upload.
```
