// To log the string in Text File, four methods are written : Info,warn,error and Debug
    public static void fnLogInfo(String strToLog) throws IOException {
        logger.info(strToLog);
    }

    public static void fnLogWarn(String strToLog) throws IOException {
        logger.warn(strToLog);
    }

    public static void fnLogError(String strToLog) throws IOException {
        logger.error(strToLog);
    }

    public static void fnLogDebug(String strToLog) throws IOException {
        logger.debug(strToLog);
    }
