# DataBase
��Ҫ����,����fork


## �����㷨
PBE + RSA 

## �����㷨

---------------- 

����: ��˾��.QQ����.����ȫ��.ĸ��ȫ��+�ҵ�С��.Base64(secretKey)
��: ����.UUID.substring(0,8);

---------------- 


```

public class DataBase {

	private static final String ALGORITHM = "PBEWITHMD5andDES";
	
	
	private static String salt = "xxxxxxxx";
	
	private static int iterationCount = 100;// ��������


	public static void main(String[] args) {

		
		String content = loadDataFrom("F:/openSource/MyGit/DataBase/README.md");
		
		
		String password = "xx.xx.xx.xx";
		
		byte[] salt  = initSalt();
		
		
		//���� 
		byte[] encriptDatas = encript(content.getBytes(), password, salt);
		String encriptBaseStr = Base64.encodeBase64String(encriptDatas);
		System.out.println(encriptBaseStr);
		
		
		//����
		byte [] encodeData = Base64.decodeBase64(encriptBaseStr);
		byte[] decrptDatas = decrypt(encodeData, password, salt);
//		System.out.println(new String(decrptDatas));
		
	}

	private static String loadDataFrom(String file) {
		try {
			File f = new File(file);
			FileReader fileReader= new FileReader(f);
			BufferedReader bufferedReader = new BufferedReader(fileReader);
			
			String line = "";
			StringBuffer buffer = new StringBuffer();
			while((line=(bufferedReader.readLine()))!=null){
				buffer.append(line+"\r\n");
			}
			bufferedReader.close();
			fileReader.close();
			
			return buffer.toString();
			
			
		} catch (Exception e) {
			e.printStackTrace();
			return null;
		}
	}

	public static byte[] initSalt() {
		
		return salt.getBytes();
		
		
	}

	public static Key getKey(String password) {
		try {
			// ��Կ����
			PBEKeySpec keySpec = new PBEKeySpec(password.toCharArray());
			// ��Կ����
			SecretKeyFactory keyFactory = SecretKeyFactory.getInstance(ALGORITHM);
			// ������Կ
			SecretKey secretKey = keyFactory.generateSecret(keySpec);
			return secretKey;

		} catch (Exception e) {
			e.printStackTrace();
			return null;
		}

	}

	// ����
	public static byte[] encript(byte[] data, String password, byte[] salt) {
		try {
			Key key = getKey(password);
			// ʵ����PBE��������
			PBEParameterSpec parameterSpec = new PBEParameterSpec(salt, iterationCount);
			Cipher cipher = Cipher.getInstance(ALGORITHM);
			cipher.init(Cipher.ENCRYPT_MODE, key, parameterSpec);
			return cipher.doFinal(data);
		} catch (Exception e) {
			e.printStackTrace();
			return null;
		}

	}
	
	//���� 
	public static byte[] decrypt(byte[] data,String password,byte[]salt){
		try {
			Key key = getKey(password);
			PBEParameterSpec parameterSpec = new PBEParameterSpec(salt, iterationCount);
			Cipher cipher = Cipher.getInstance(ALGORITHM);
			cipher.init(Cipher.DECRYPT_MODE, key, parameterSpec);
			return cipher.doFinal(data);
		} catch (Exception e) {
			e.printStackTrace();
			return null;
		}
		
		
	}
	
	
	

}

```