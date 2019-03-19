---
title: Jsoup 处理 XSS
tags: [Java]
created: '2019-03-12T03:35:31.459Z'
modified: '2019-03-12T03:38:31.124Z'
---

# Jsoup 处理 XSS

[XSS 过滤需要后端处理](https://www.v2ex.com/t/543559#reply13)，Jsoup 有很多预定义好的白名单供选择，也可以自己定义白名单

```java
public static Whitelist relaxed() {
    return new Whitelist()
        .addTags(
            "a", "b", "blockquote", "br", "caption", "cite", "code", "col",
            "colgroup", "dd", "div", "dl", "dt", "em", "h1", "h2", "h3", "h4", "h5", "h6",
            "i", "img", "li", "ol", "p", "pre", "q", "small", "span", "strike", "strong",
            "sub", "sup", "table", "tbody", "td", "tfoot", "th", "thead", "tr", "u", "ul"
        )

        .addAttributes("a", "href", "title")
        .addAttributes("blockquote", "cite")
        .addAttributes("col", "span", "width")
        .addAttributes("colgroup", "span", "width")
        .addAttributes("img", "align", "alt", "height", "src", "title", "width")
        .addAttributes("ol", "start", "type")
        .addAttributes("q", "cite")
        .addAttributes("table", "summary", "width")
        .addAttributes("td", "abbr", "axis", "colspan", "rowspan", "width")
        .addAttributes("th", "abbr", "axis", "colspan", "rowspan", "scope", "width")
        .addAttributes("ul", "type")

        .addProtocols("a", "href", "ftp", "http", "https", "mailto")
        .addProtocols("blockquote", "cite", "http", "https")
        .addProtocols("cite", "cite", "http", "https")
        .addProtocols("img", "src", "http", "https")
        .addProtocols("q", "cite", "http", "https");
}
```

代码调用示例:

```js
import org.apache.commons.lang3.StringUtils;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.safety.Whitelist;

public class JsoupUtil {

    private static final Whitelist WHITELIST = Whitelist.relaxed();

    static {
        // 富文本编辑时一些样式是使用 style 来进行实现的
        // 比如红色字体 style="color:red;"
        // 所以需要给所有标签添加 style 属性
        WHITELIST.addAttributes(":all", "style");
        WHITELIST.addAttributes(":all", "class");
        WHITELIST.addAttributes(":all", "target");
        WHITELIST.addAttributes(":all", "spellcheck");
    }

    private JsoupUtil() {}

    private static final Document.OutputSettings OUTPUT_SETTINGS = new Document.OutputSettings().prettyPrint(false);

    public static String clean(String content) {
        if (StringUtils.isBlank(content)) {
            return "";
        }
        return Jsoup.clean(content, "", WHITELIST, OUTPUT_SETTINGS);
    }
}
```
