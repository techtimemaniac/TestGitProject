public class DateUtils {
    public static String todayDate(String format) throws ParseException {
        Date date = Calendar.getInstance().getTime();
        // Display a date in day, month, year format
        DateFormat formatter;
        if (format == "")
            formatter = new SimpleDateFormat("dd/mm/yyyy");
        else
            formatter = new SimpleDateFormat(format);
        String today = formatter.format(date);
        return today;
    }

    public static Date yesterday() {
        final Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DATE, -1);
        return cal.getTime();
    }

    public static String getYesterdayDateString(String format) {
        DateFormat dateFormat = new SimpleDateFormat(format);
        return dateFormat.format(yesterday());
    }

    private static Date eweek() {
        final Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DATE, -6);
        return cal.getTime();
    }

    public static String getweekDateString(String format) {
        DateFormat dateFormat = new SimpleDateFormat(format);
        return dateFormat.format(eweek());
    }

    private static Date fourweek() {
        final Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DATE, -27);
        return cal.getTime();
    }

    public static String getfourweekDateString(String format) {
        DateFormat dateFormat = new SimpleDateFormat(format);
        return dateFormat.format(fourweek());
    }
