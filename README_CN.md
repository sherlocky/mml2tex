## 修改内容
- 1.修改了一些xsl文件的引用地址；
- 2.不存在的地址，手动下载依赖的xsl文件并替换了云端地址。
> 地址 http://transpect.io/xslt-util/colors/xsl/ 停服了，
> 从开源项目 https://github.com/transpect/xslt-util 下载xsl文件。

## 使用命令
```bash
cd /mml2tex
#cp saxon9he-9.4.0.4.jar

java -jar saxon9he-9.4.0.4.jar -s:example/aspose.xml -xsl:xsl/invoke-mml2tex.xsl
#\sqrt{x^{2}+y^{2}}
#<?xml version="1.0" encoding="UTF-8"?><?mml2tex \sqrt{x^{2}+y^{2}}?>%

java -jar saxon9he-9.4.0.4.jar -s:example/aspose2.xml -xsl:xsl/invoke-mml2tex.xsl
#x=\frac{-b\pm \sqrt{b^{2}-4\mathit{ac}}}{2a}
#<?xml version="1.0" encoding="UTF-8"?><?mml2tex x=\frac{-b\pm \sqrt{b^{2}-4\mathit{ac}}}{2a}?>%

java -jar saxon9he-9.4.0.4.jar -s:example/aspose3.xml -xsl:xsl/invoke-mml2tex.xsl
#C_{6}H_{12}O_{6}+6O_{2}\frac{\underline {~ ~ ~ ~ ~ ~ ~ ~ ~ ~ 酶~ ~ ~ ~ ~ ~ ~ ~ ~ ~ }}{~ }6CO_{2}+6H_{2}O
#<?xml version="1.0" encoding="UTF-8"?><?mml2tex C_{6}H_{12}O_{6}+6O_{2}\frac{\underline {~ ~ ~ ~ ~ ~ ~ ~ ~ ~ 酶~ ~ ~ ~ ~ ~ ~ ~ ~ ~ }}{~ }6CO_{2}+6H_{2}O?>%

java -jar saxon9he-9.4.0.4.jar -s:example/aspose4.xml -xsl:xsl/invoke-mml2tex.xsl
#2H_{2}+\text{O}_{2}\xrightleftharpoons{燃烧/电解}\text{2H}_{2}O
```

> 使用以下两个版本都可以：
```xml
<!-- https://mvnrepository.com/artifact/net.sf.saxon/saxon9he -->
<dependency>
    <groupId>net.sf.saxon</groupId>
    <artifactId>saxon9he</artifactId>
    <version>9.4.0.4</version>
</dependency>

<dependency>
  <groupId>net.sf.saxon</groupId>
  <artifactId>Saxon-HE</artifactId>
  <version>9.6.0-5</version>
</dependency>
```

## Java 代码调用
```java
        /**
         * 参考 https://github.com/transpect/mml2tex
         * net.sf.saxon.Transform 类
         */
        String mmlFilePath = "mml2tex/example/aspose4.xml";
        //TODO 上线时位置要改一下
        String transformXsl = "mml2tex/xsl/invoke-mml2tex.xsl";
        String latexFilePath = "mml2tex/example/aspose4.latex.xml";
        String[] transformParam = new String[] {
                "-s:" + mmlFilePath,
                "-xsl:" + transformXsl,
                "-o:" + latexFilePath};
        try {
            File file = new File(latexFilePath);
            try {
                FileUtil.del(file);
            } catch (Exception e) {
                //ignore
            }
            net.sf.saxon.Transform.main(transformParam);
            if (!file.isFile() || !file.exists() || file.length() <= 0) {
                return;
            }
            String latex = FileUtil.readUtf8String(file);
            if (StringUtils.isBlank(latex)) {
                return;
            }
            String prefix = "<?xml version=\"1.0\" encoding=\"UTF-8\"?><?mml2tex ";
            int start = prefix.length();
            //String suffix = "?>";
            int end = latex.length() - 2;
            latex = StringUtils.substring(latex, start, end);
            System.out.println(latex);
        } catch (Exception e) {
            System.out.println("error:" + e.getMessage());
        }
```