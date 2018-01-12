# -内容太少，直接看描述文件就行。

    public static void main(String[] args) {
        String str = "我是谁 who am I";
        String x2 = stringToX2(str);
        System.out.println("转化为二进制字符串为：" + x2);
        String resultStr = null;
        try {
            resultStr = x2ToString(x2);
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        System.out.println("二进制转化为字符串：" + resultStr);
    }

    public static String stringToX2(String str){
        StringBuilder sb = new StringBuilder();
        byte[] bytes = new byte[0];
        try {
            bytes = str.getBytes("utf-8");
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        for (byte b : bytes){
            byte[] bs = new byte[8];
            for (int i = 0; i < 8; i++) {
                bs[i] = (byte)(((1<<(7-i)&b)==0)?0:1);
            }
            for (byte c : bs){
                sb.append(c);
            }
        }
        return sb.toString();
    }

    public static String x2ToString(String str) throws UnsupportedEncodingException {
        int start = 0;
        int end = start + 8;
        int l = str.length();
        byte[] bb = new byte[l/8];
        int j = 0;
        while(start < l){
            String temp = str.substring(start,end);
            char[] heex = temp.toCharArray();
            byte[] bytes = new byte[heex.length];
            int i = 0;
            for (char a: heex) {
                byte[] b = new byte[1];
                b[0] = (byte) (a- 48);
                bytes[i] = b[0];
                i++;
            }
            String origin =  "";
            for (byte b1:bytes ) {
                origin = origin + b1;
            }
            byte bh =(byte) Integer.parseInt(origin,2);
            bb[j] = bh;
            j++;
            start = end;
            end = end + 8;
            if(end >= str.length()){
                end = str.length();
            }
        }
        String result =  new String(bb,"utf-8");
        return result;
    }
