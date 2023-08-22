# socket 网络连接
```
    public static void main(String[] args) throws IOException {
        try (Socket s = new Socket("time-a.nist.gov", 13);
             Scanner in = new Scanner(s.getInputStream(), "UTF-8");) {
            while (in.hasNextLine()) {
                String s1 = in.nextLine();
                System.out.println(s1);
            }

        }
    }
```