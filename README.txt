{0=��Դ���, 1=���, 2=��ͬ���, 3=��Ŀ����}

0=��Դ���
1=���
2=��ͬ���
3=��Ŀ����

```
(?<={*)(\d=)([^(,|})]*)

\d=([^(,+)}]*)

\d=[^,}]*
```

> ^��ƥ�䶺�ţ���ƥ�䵽\d ����ƥ��

------

String urls[] = new String[]{
                "http://192.168.100.238/svn/acssvn/Control/dev/Code/trunk",
                "http://192.168.100.238/svn/acssvn/Control/dev/Code/branches",
                "http://192.168.100.238/svn/acssvn/Control/dev/Code/branches/tags20190516/xycnqs",

?				"http://192.168.100.238/svn/acssvn/Control/dev/Code/iteration/20191031/aaa.java",
?          	  "http://192.168.100.238/svn/acssvn/Control/dev/Code/iteration/20191031/com.yss.acs.fundacc",
?            "http://192.168.100.238/svn/acssvn/Control/dev/Code/iteration/20191031/com.yss.acs.fundacc.rule",

?        "http://192.168.100.238/svn/acssvn/Control/cgb/Code/trunk",
?        "http://192.168.100.238/svn/acssvn/Control/cgb/Code/dev20190802003",

?        "http://192.168.100.238/svn/acssvn/Control/cmbc/Code/branches",
?        "http://192.168.100.238/svn/acssvn/Control/gfsc/Code/branches/tags201903/002_tags/002_kcb/com.yss.acs.interface",
?        "http://192.168.100.238/svn/acssvn/Control/gfsc/Code/branches/tags201903/002_tags/002_kcb/com.yss.acs.fundacc",
?        "http://192.168.100.238/svn/acssvn/Control/gfsc/Code/branches/tags201903/002_tags/002_kcb/com.yss.acs.fundacc.rule",

?        "http://192.168.100.238/svn/CodeCheck",
?        "http://192.168.100.238/svn/CodeCheck/Test/asds.java",

?        "http://192.168.100.238/svn/acssvn/Control/gfsc/Code/branches/a",
?        "http://192.168.100.238/svn/acssvn/Control/gfsc/Code/",

?        "http://192.168.100.238/svn/acssvn/Control/dev/Code/branches/tags20190516/003/checking/com.yss.acs.checking.core",
?        "http://192.168.100.238/svn/acssvn/Control/dev/Code/branches/tags20190516/003/checking/com.yss.acs.checking.core/a",

?        "http://192.168.100.238/svn/acssvn/Control/gfsc/Code/branches/tags201903/003",

?        "http://192.168.100.238/svn/acssvn/Control/cmbc/Code/branches/tags20190930",

?        "http://219.141.235.67:20080/svn/acssvn/Trunk/SourceCode/21GZ3.5/project_gz3.5/fundacc3"
};

��Ŀ��ȥ������cmbc/Code/trunk/code��|gfsc/Code/branches/tags201903/003·��



```
(?!http://(\d*\.){3}\d*.*gfsc/Code/branches/tags201903/003[^,"]*)http://(\d*\.){3}\d*(:\d+)*(\/[^\/",]+)*

(?!http://(\d*\.\d*(:\d*)*)*.*gfsc/Code/branches/tags201903/003[^",]*)http://(\d*\.\d*(:\d*)*)*[^",]*

(?<=.*\")((?<!gfsc).)*(?=\")
(?<=\")((?<!gfsc).)*(?=\")
```

