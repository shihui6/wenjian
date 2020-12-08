***XML
    1.什么是XML
        XML是可扩展的标记性语言
    2.xml的作用
        1.用来保存数据，而且这些数据具有自我描述性
        2。它可以作为项目或者模块的配置文件
        3.还可以作为网络传输数据的格式(现在以JSON为主)
    
    3.xml语法：
        1.文档声明
            事例：事例说明解释

                ```xml
                    <?xml version="1.0" encoding="utf-8" ?>
                    <!--
                        <?xml version="1.0" encoding="utf-8" ?>
                        以上内容是xml文件声明，表示下面的文件是以xml格式来写的，用xml解析器解析解析下面的内容
                        version 表示xml的版本
                        encoding 表示xml文件本身的编码
                    -->
                    <books>
                        <book>
                            <name>时间简史</name>
                            <author>霍金</author>
                            <price>75</price>
                        </book>
                    </books>
                ```
        2.元素(标签)
            xml元素(和html里面的标签一个意思)必须遵守以下命名规则
                1.名称可以含字母，数字以及其它字符
                2.名称不能以数字或者标点符号开始
                3.名称不能包含空格
                4.文档必须有根元素
                    根元素就是顶级元素
                    没有父标签的元素，叫顶级元素
                    根元素是没有父标签的顶级元素，而且是唯一一个才行
                5.xml的属性值必须加引号
                6.xml中支持特殊字符(如：&gt,&lt)
                7.文本区域(CDATA区)
                    CDATA语法可以告诉xml解析器，我CDATA里的文本内容，只是纯文本，不需要xml语法解析
                    CDATA格式
                        <![CDATA[ 这里可以把你输入的字符原样显示，不会解析xml ]]>

                        事例：文本区域使用<![CDATA[]]>

                            ```xml
                                <books>
                                    <book>
                                        <name>时间简史</name>
                                        <author>霍金</author>
                                        <price>
                                            <![CDATA[
                                                <<<今天天气真好><>>
                                            ]]>
                                        </price>
                                    </book>
                                </books>
                            ```

            注意点：xml解析器对大小写敏感

        3.xml属性
            xml的标签属性和html的标签属性是非常的类似的，属性可以提供元素的额外信息
            在标签上可以书写属性
                一个标签上可以书写多个属性。每个属性的值必须使用引号引起来
    
    4.xml解析技术介绍
        xml是可扩展的标记语言
            不管是html文件还是xml文件它们都是标记型文档，都可以使用w3c组织指定的dom技术来解析(dom技术就是将标签解析成document对象)

        解析dom的技术
            Dom4j解析技术：概念：它是第三方的解析技术。我们需要使用第三方给我们提供好的类库才可解析xml文件
            Dom4j解析技术的作用：读取xml中的数据，转化成我们所需要的内容，这就叫做解析
        
        实际操作：通过dom4j解析xml文件，通过junit进行单元测试

            ```java
                @Test //单个方法测试需要用到junit jar包
                public void test2() throws Exception {
                    //1.读取books.xml
                    //创建一个SaxReader输入流，读取xml配置文件，生成doucument对象(这里需要dom4j的jar包)
                    SAXReader reader = new SAXReader();
                        //在junit测试中，相对路径是从模块名src开始算
                    Document document = reader.read("src/books.xml");
                    //2.通过document对象获取根元素
                    Element rootElement = document.getRootElement();
                    //3.通过根元素获取book标签对象
                        //elemet()和elements()都是通过标签名查找子元素
                    List<Element> books = rootElement.elements("book");
                    //4.遍历，处理每个book标签转换为book类
                    for(Element book:books){
                        //asXML()把标签对象，转化为标签字符串
                        Element nameElement = book.element("name");
                        //getText():可以获取标签中的文本内容
                        String nametext = nameElement.getText();
                        String pricetext = book.elementText("price");
                        String authortext = book.elementText("author");
                        String sn = book.attributeValue("sn");
                        System.out.println(new Book(sn,nametext,Double.parseDouble(pricetext),authortext));
                    }
                }
            ```

        
