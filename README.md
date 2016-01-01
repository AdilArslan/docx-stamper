# docx-stamper
docx-stamper is a template engine for docx documents. You create a template .docx document with your favorite word processor
and feed it to a DocxStamper instance to create a document based on the template at runtime. Example code:
```java
MyContext context = ...;                 // your own POJO against which expressions found in the template
                                         // will be resolved
InputStream template = ...;              // InputStream to your .docx template file
OutputStream out = ...;                  // OutputStream in which to write the resulting .docx document
DocxStamper stamper = new DocxStamper();
stamper.stamp(template, context, out);
out.close();
```

## Replacing Expressions in a .docx Template
The main feature of docx-stamper is **replacement of expressions** within the text of the template document. Simply add expressions like `${person.name}` in the text of your .docx template and provide a context object against which the expression can be resolved. docx-stamper will try to keep the original formatting of the text in the template intact.

The value an expression resolves to may be of the following types:

| Type of expression value | Effect  |
| ---|---|
| java.lang.Object | The expression is replaced by the String representation of the object (`String.valueOf()`).
| java.lang.String | The expression is replaced with the String value.|
| java.util.Date   | The expression is replaced by a formatted Date string (by default "dd.MM.yyyy"). You can change the format string by registering your own DateResolver (see below).|
| org.wickedsource.docxstamper.replace.typeresolver.image.Image | The expression is replaced with an inline image.|

If an expression cannot be resolved successfully, it will be skipped (meaning the expression stays in the document as it was in the template). To support more than the above types you can implement and register your own [TypeResolver](http://thombergs.github.io/docx-stamper/apidocs/org/wickedsource/docxstamper/api/typeresolver/ITypeResolver.html).

## Further Manipulation of the .docx Template
Besides replacing expressions, docx-stamper can process comments on paragraphs of text in the template .docx document and do manipulations on the template based on these comments. By default, you can use the following expressions in comments:

| Expression in .docx comment       | Effect  |
| --------------------------------- |---------|
| `displayParagraphIf(boolean)`     | The commented paragraph is only displayed in the resulting .docx document if the boolean condition resolves to `true`.|
| `displayTableRowIf(boolean)`      | The table row surrounding the commented paragraph is only displayed in the resulting .docx document if the boolean condition resolves to `true`.|
| `displayTableIf(boolean)`      | The whole table surrounding the commented paragraph is only displayed in the resulting .docx document if the boolean condition resolves to `true`.|
| `repeatTableRow(List<Object>)`      | The table row surrounding the commented paragraph is copied once for each object in the passed-in list. Expressions found in the cells of the table row are evaluated against the object from the list.

If a comment cannot be processed, it is simply skipped. Successfully processed comments are removed from the document. You can add support to more expressions in comments by implementing and registering your own [ICommentProcessor](http://thombergs.github.io/docx-stamper/apidocs/org/wickedsource/docxstamper/api/commentprocessor/ICommentProcessor.html).

# Maven coordinates
to be done



