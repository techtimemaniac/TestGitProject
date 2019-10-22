public class DataMapUtils {

    private static String datamapingXlsxPath;

    @Value("${datamapping.xlsx.path}")
    public void setDatamapingXlsxPath(String path) { datamapingXlsxPath = path;}

    public static DataMapper getFromData(String fileName) {
        String directoryPath = System.getProperty("user.dir");
        String filePath= directoryPath + "/" + datamapingXlsxPath + fileName + ".xlsx";
        DataMapper FromData = new XLSXDataMapper(filePath);
        return FromData;
    }
