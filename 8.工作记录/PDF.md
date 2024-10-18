PDFBox
Itext5 免费  7商用收费  适用生成PDF以及对PDF进行操作
iText Pdf2data 根据模版解析数据
Stirling-PDF docker部署本地使用,支持web操作，定制化功能少
aspose-pdf 转换为word

```java 
import java.io.File;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import org.apache.pdfbox.contentstream.PDFStreamParser;
import org.apache.pdfbox.cos.COSName;
import org.apache.pdfbox.cos.COSStream;
import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.pdmodel.PDPage;
import org.apache.pdfbox.pdmodel.graphics.PDXObject;
import org.apache.pdfbox.pdmodel.graphics.image.PDImageXObject;
import org.apache.pdfbox.text.PDFGraphicsStreamEngine;
import org.apache.pdfbox.text.PDFStreamEngine;
import org.apache.pdfbox.text.TextPosition;
import org.apache.pdfbox.util.Matrix;

/*
PDFBox获取最顶端的图片
*/

public class ExtractTopImageFromPDF extends PDFGraphicsStreamEngine {

    private PDImageXObject topImage = null;
    private float topY = Float.MAX_VALUE;  // Y坐标值，越小越靠近页面顶部

    public ExtractTopImageFromPDF(PDPage page) {
        super(page);
    }

    @Override
    public void drawImage(PDImageXObject image) throws IOException {
        // 获取当前图片的矩阵信息
        Matrix ctm = getGraphicsState().getCurrentTransformationMatrix();
        float y = ctm.getTranslateY();  // 获取图片的Y坐标

        // 如果图片更靠近页面顶部，则记录该图片
        if (y < topY) {
            topY = y;
            topImage = image;
        }
    }

    public PDImageXObject getTopImage() {
        return topImage;
    }

    public static PDImageXObject extractTopImageFromPage(PDDocument document, int pageIndex) throws IOException {
        PDPage page = document.getPage(pageIndex);
        ExtractTopImageFromPDF extractor = new ExtractTopImageFromPDF(page);
        extractor.processPage(page);
        return extractor.getTopImage();
    }

    public static void main(String[] args) {
        try (PDDocument document = PDDocument.load(new File("your-pdf-file.pdf"))) {
            int pageIndex = 0;  // 获取第一页的图片
            PDImageXObject topImage = extractTopImageFromPage(document, pageIndex);

            if (topImage != null) {
                System.out.println("Found top image on page " + pageIndex);
                // 你可以将图片保存到文件
                topImage.getImage().writeTo(new File("top-image.png"));
            } else {
                System.out.println("No images found on page " + pageIndex);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```