public class XLSXDataMapper implements DataMapper {
    private Object dataMapSyncLock = new Object();
    private HashMap<String, Object> dataMap;
    private String dataValue;
    private String filePath;

    public XLSXDataMapper(String filePath) {
        this.filePath = filePath;
    }

    private XLSXDataMapper(Boolean isDataValue, String dataValue) {
        this.dataValue = dataValue;
    }

    private XLSXDataMapper(HashMap<String, Object> dataMap) {
        this.dataMap = dataMap;
    }

    private void loadData() {
        if (filePath != null) {
            String dataFilePath = filePath;
            if (dataFilePath != null && !dataFilePath.trim().equals("")) {
                try {
                    Workbook wb = WorkbookFactory.create(new File(dataFilePath));
                    HashMap<String, Object> topDataMap = new HashMap();
                    Iterator var6 = wb.iterator();
                    if (var6.hasNext()) {
                        Sheet sheet = (Sheet)var6.next();
                        List<String> keyList = new ArrayList();
                        Iterator var9 = sheet.iterator();

                        while(var9.hasNext()) {
                            Row row = (Row)var9.next();
                            HashMap<String, Object> currentDataMap = topDataMap;
                            String currentKey = "";

                            try {
                                int valueIndex = this.discoverValueIndex(row);
                                if (valueIndex != -1) {
                                    HashMap<String, Object> dataMapBeforeValue = new HashMap();

                                    for(int cn = 0; cn < row.getLastCellNum() && cn <= valueIndex; ++cn) {
                                        String cellValue = "";
                                        Cell cell = row.getCell(cn);
                                        if (cell != null) {
                                            CellType cType = cell.getCellTypeEnum();
                                            switch(cType) {
                                                case STRING:
                                                    cellValue = cell.getStringCellValue();
                                                    break;
                                                case NUMERIC:
                                                    cellValue = String.valueOf(cell.getNumericCellValue());
                                                    break;
                                                case BOOLEAN:
                                                    cellValue = String.valueOf(cell.getBooleanCellValue());
                                                    break;
                                                case ERROR:
                                                    cellValue = String.valueOf(cell.getErrorCellValue());
                                                    break;
                                                case FORMULA:
                                                    cellValue = String.valueOf(cell.getCellFormula());
                                                    break;
                                                default:
                                                    cellValue = cell.getStringCellValue();
                                            }
                                        }

                                        if (cn < valueIndex) {
                                            if (cellValue.trim().equals("") && cn < ((List)keyList).size()) {
                                                currentKey = (String)((List)keyList).get(cn);
                                                if (cn == valueIndex - 1) {
                                                    dataMapBeforeValue = currentDataMap;
                                                }

                                                currentDataMap = (HashMap)currentDataMap.get(currentKey);
                                            } else if (!cellValue.trim().equals("") && cn < ((List)keyList).size()) {
                                                if (!((String)((List)keyList).get(cn)).trim().equals(cellValue.trim())) {
                                                    keyList = ((List)keyList).subList(0, cn);
                                                    ((List)keyList).add(cn, cellValue.trim());
                                                    currentKey = cellValue.trim();
                                                } else {
                                                    currentKey = (String)((List)keyList).get(cn);
                                                }

                                                if (currentDataMap.get(currentKey) == null) {
                                                    currentDataMap.put(currentKey, new HashMap());
                                                }

                                                if (cn == valueIndex - 1) {
                                                    dataMapBeforeValue = currentDataMap;
                                                }

                                                currentDataMap = (HashMap)currentDataMap.get(currentKey);
                                            } else {
                                                ((List)keyList).add(cn, cellValue.trim());
                                                currentKey = cellValue.trim();
                                                if (currentDataMap.get(currentKey) == null) {
                                                    currentDataMap.put(currentKey, new HashMap());
                                                }

                                                if (cn == valueIndex - 1) {
                                                    dataMapBeforeValue = currentDataMap;
                                                }

                                                currentDataMap = (HashMap)currentDataMap.get(currentKey);
                                            }
                                        } else if (cn == valueIndex) {
                                            dataMapBeforeValue.put(currentKey, cellValue.trim().equalsIgnoreCase("$empty") ? "" : cellValue);
                                        }
                                    }
                                }
                            } catch (Exception var18) {
                                var18.printStackTrace();
                            }
                        }
                    }

                    this.dataMap = topDataMap;
                } catch (Exception var19) {
                    var19.printStackTrace();
                    this.dataMap = new HashMap();
                }
            } else {
                this.dataMap = new HashMap();
            }
        } else {
            this.dataMap = new HashMap();
        }

    }

    private int discoverValueIndex(Row row) {
        int currIndex = -1;

        for(int cn = 0; cn < row.getLastCellNum(); ++cn) {
            Cell cell = row.getCell(cn);
            if (cell != null) {
                CellType cType = cell.getCellTypeEnum();
                String cellValue = "";
                switch(cType) {
                    case STRING:
                        cellValue = cell.getStringCellValue();
                        break;
                    case NUMERIC:
                        cellValue = String.valueOf(cell.getNumericCellValue());
                        break;
                    case BOOLEAN:
                        cellValue = String.valueOf(cell.getBooleanCellValue());
                        break;
                    case ERROR:
                        cellValue = String.valueOf(cell.getErrorCellValue());
                        break;
                    case FORMULA:
                        cellValue = String.valueOf(cell.getCellFormula());
                        break;
                    default:
                        cellValue = cell.getStringCellValue();
                }

                if (!cellValue.trim().equals("")) {
                    currIndex = cn;
                }

                if (cellValue.trim().equalsIgnoreCase("$empty")) {
                    break;
                }
            }
        }

        return currIndex;
    }

    public DataMapper Key(String key) {
        Object v;
        if (this.dataMap == null) {
            v = this.dataMapSyncLock;
            synchronized(this.dataMapSyncLock) {
                if (this.dataMap == null) {
                    this.loadData();
                }
            }
        }

        v = this.dataMap.get(key);
        if (v == null) {
            return new XLSXDataMapper(new HashMap());
        } else {
            return v.getClass() == String.class ? new XLSXDataMapper(true,(String)v) : new XLSXDataMapper((HashMap)v);
        }
    }

    public DataMapper then() {
        return this;
    }

    public String Value() {
        if (this.dataValue == null && this.dataMap == null) {
            Object var1 = this.dataMapSyncLock;
            synchronized(this.dataMapSyncLock) {
                if (this.dataMap == null) {
                    this.loadData();
                }
            }
        }

        return this.dataValue == null ? "" : this.dataValue;
    }
