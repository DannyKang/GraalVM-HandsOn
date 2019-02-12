# GraalVM Hands On

## Graalvm.org에 있는 Getting Started를 따라서 설치

- from : https://www.graalvm.org/docs/getting-started/

### **STEP 1**: Hands On 환경 구성하기

<details>
<summary>환경 구성하기</summary>
#### 직접 설치하기 
 - from : https://www.graalvm.org/docs/getting-started/
  
 1. [Download](https://www.graalvm.org/downloads/) 페이지에서, CE(Commity Edition) 버전이나 EE(Enterprise Edition) 버전의 다운로드 받는다.
 2. 압축파일을 적당한 위치에 풀고 PATH에 GraalVM/bin을 추가한다.

  - Linux
 ``` bash
 export PATH=/path/to/graalvm/bin:$PATH
 ```
 
  - Mac On
 ``` bash
 export PATH=/path/to/graalvm/Contents/Home/bin:$PATH
 ```
 
#### Docker Image 내려받기

Image 크기가 약 1.44G이기 때문에 시간이 시간이 소요된다.

``` bash 
docker pull oracle/graalvm-ce:1.0.0-rc11
```
</details>


***


### **STEP 2**: 설치 확인 
#### java, node, lli, ruby 버전 확인하기 



You can verify whether you are using GraalVM with the echo command: echo $PATH.
Optionally set the JAVA_HOME environment variable to resolve to the GraalVM installation directory. You can also specify GraalVM as the JRE or JDK installation in your Java IDE.

GraalVM’s /bin directory is similar to that of a standard JDK, but includes a set of additional launchers:

js runs a JavaScript console with GraalVM.
node is a drop-in replacement for Node.js, using GraalVM’s JavaScript engine.
lli is a high-performance LLVM bitcode interpreter integrated with GraalVM.
native-image builds an ahead-of-time compiled executable or a shared library from Java applications.
gu (Graal updater) can be used to install language packs for Python, R, and Ruby.
Notably, java in GraalVM runs the JVM with Graal as the default compiler. The Ruby, Python and R executables become available only if you install the corresponding language engines. For example, running the following command will install Ruby support:

gu install ruby
For more information on using Graal updater please refer to its documentation.

Once the PATH environment variable is set properly, it’s easy to check language versions with GraalVM launchers:

$ java -version
java version "1.8.0_192"
Java(TM) SE Runtime Environment (build 1.8.0_192-b12)
GraalVM 1.0.0-rc12 (build 25.192-b12-jvmci-0.54, mixed mode)

$ node -v
v10.15.0

$ lli --version
Graal llvm 6.0.0 (GraalVM EE Native 1.0.0-rc12)
If you have, e.g., the Ruby language component installed, you can check its version as well:

$ ruby -v
truffleruby 1.0.0-rc12, like ruby 2.4.4, GraalVM CE Native [x86_64-darwin]

***

### **STEP 3**: 간단히 예제 실행해 보기 

#### 언어별 예제 실행해 보기 
<details>
<summary>언어별 예제 </summary>
Running Applications
Since the executables of all language runtimes in GraalVM emulate the behavior of the languages’ default runtimes, putting GraalVM on your PATH should be enough to run your applications with GraalVM.

Running Java
Take a look at this typical HelloWorld class:

public class HelloWorld {
  public static void main(String[] args) {
    System.out.println("Hello, World!");
  }
}
Run the following command to compile this class to bytecode and then run it on GraalVM:

$ javac HelloWorld.java
$ java HelloWorld
Hello World!
You can find a collection of larger examples of Java applications you can try running with GraalVM on the examples page.

Running JavaScript
GraalVM can execute plain JavaScript code, both in REPL mode and by executing script files directly.

$ js
> 1 + 2
3
GraalVM also supports running Node.js applications. More than 45.000 npm modules are tested and are fully compatible with GraalVM, including modules like express, react, async, request, browserify, grunt, mocha, and underscore. To install a Node.js module, use the npm executable in the /bin folder of the GraalVM package. The npm command is equivalent to the default Node.js command and supports all Node.js APIs.

1. Install the colors and ansispan modules using npm install:

npm install colors ansispan
After the modules are installed, you can use them from your application.

2. Add the following code snippet to a file named app.js and save it in the same directory where you installed the Node.js modules:

Copy
const http = require("http");
const span = require("ansispan");
require("colors");

http.createServer(function (request, response) {
    response.writeHead(200, {"Content-Type": "text/html"});
    response.end(span("Hello Graal.js!".green));
}).listen(8000, function() { console.log("Graal.js server running at http://127.0.0.1:8000/".red); });
3. Execute it on GraalVM using the node command:

node app.js
More information on compatibility with the Node.js and configuring GraalVM read the reference manual on JavaScript in GraalVM.

Running LLVM Interpreter
The LLVM interpreter in GraalVM executes LLVM bitcode, and therefore any programming language that can be compiled to LLVM bitcode.

Put this C HelloWorld into a file named hello.c:

#include <stdio.h>

int main() {
    printf("Hello from GraalVM!\n");
    return 0;
}
You can compile hello.c to an LLVM bitcode file named hello.bc using the following command:

$ clang -c -O1 -emit-llvm hello.c
You can then run hello.bc on GraalVM like this:

$ lli hello.bc
Hello from GraalVM!
More examples and information on what languages and runtimes are supported can be found in the reference manual for LLVM.

Running Ruby
The Ruby engine is not installed by default, but it can easily be added using the Graal updater:

gu install ruby
This makes Ruby commands like ruby, gem, irb, rake, rdoc and ri available:

$ ruby [options] program.rb
GraalVM’s Ruby implementation uses the same options as the standard implementation of Ruby, with some additions.

$ gem install chunky_png
$ ruby -r chunky_png -e "puts ChunkyPNG::Color.to_hex(ChunkyPNG::Color('mintcream @ 0.5'))"
#f5fffa80
Using Bundler
The only supported version of Bundler at the moment is 1.16.x.

$ gem install bundler -v 1.16.3
$ bundle exec ...
More examples and additional information on Ruby support in GraalVM can be found in the reference manual for Ruby.

Running R
The R engine is not installed by default, but it can easily be added using the Graal updater:

gu install r
When the R engine is installed, you can execute R scripts and use the R REPL with GraalVM:

$ R
R version 3.5.1 (FastR)
...

> 1 + 1
[1] 2
More examples and additional information on R support in GraalVM can be found in the reference manual for R.

Running Python
GraalVM implementation of Python 3.7 has recently been started. The Python engine is not installed by default, but it can easily be added using the Graal updater:

gu install python
Once the Python engine is installed, GraalVM can execute Python programs:

$ graalpython
...
>>> 1 + 2
3
>>> exit()
More examples and additional information on Python support in GraalVM can be found in the reference manual for Python.

Combine Languages
If enabled, using the --polyglot flag, scripts executed on GraalVM can use interoperability features to call into other languages and exchange data with them.

For example, running js --jvm --polyglot example.js executes example.js in a polyglot context. If the program calls any code in other supported languages, GraalVM executes that code in the same runtime as the example.js application. For more information on polyglot applications see the polyglot documentation.

Native Images
GraalVM can compile Java bytecode into native images to achieve faster startup and smaller footprint for your applications.

Let’s use the HelloWorld example from above to demonstrate how to compile Java bytecode into a native image:

// HelloWorld.java
public class HelloWorld {
  public static void main(String[] args) {
    System.out.println("Hello, World!");
  }
}
Run the following to compile the class to bytecode and then build a native image:

$ javac HelloWorld.java
$ native-image HelloWorld
This builds an executable file named helloworld in the current working directory. Invoking it executes the natively compiled code of the HelloWorld class as follows:

$ ./helloworld
Hello, World!

</details>


### **STEP 4**: Polyglot 환경 실습 

Polyglot Capabilities of Native Images
The native-image tool also makes it easy to use polyglot capabilities. Take this example of a JSON pretty-printer using the GraalVM implementation of JavaScript:

// PrettyPrintJSON.java
import java.io.*;
import java.util.stream.*;
import org.graalvm.polyglot.*;

public class PrettyPrintJSON {
  public static void main(String[] args) throws java.io.IOException {
    BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
    String input = reader.lines().collect(Collectors.joining(System.lineSeparator()));
    try (Context context = Context.create("js")) {
      Value parse = context.eval("js", "JSON.parse");
      Value stringify = context.eval("js", "JSON.stringify");
      Value result = stringify.execute(parse.execute(input));
      System.out.println(result.asString());
    }
  }
}
The --language:js argument ensures that the JavaScript engine is available in the generated image:

$ javac PrettyPrintJSON.java
$ native-image --language:js PrettyPrintJSON
This native executable can now perform JSON pretty printing:

$ ./prettyprintjson <<EOF
{"GraalVM":{"description":"Language Abstraction Platform","supports":["combining languages","embedding languages","creating native images"],"languages": ["Java","JavaScript","Node.js", "Python", "Ruby","R","LLVM"]}}
EOF
Here is the JSON output from the native executable:

{
  "GraalVM": {
    "description": "Language Abstraction Platform",
    "supports": [
      "combining languages",
      "embedding languages",
      "creating native images"
    ],
    "languages": [
      "Java",
      "JavaScript",
      "Node.js",
      "Python",
      "Ruby",
      "R",
      "LLVM"
    ]
  }
}
The native image is much faster than running the same code on the JVM directly:

$ time bin/java PrettyPrintJSON < test.json > /dev/null
real	0m1.101s
user	0m2.471s
sys	0m0.237s

$ time ./prettyprintjson < test.json > /dev/null
real	0m0.037s
user	0m0.015s
sys	0m0.016s
There are certain limitations of ahead-of-time compilation with GraalVM, listed here.