
	public void insert()throws Exception{

		String url=readPro("url");
		log.info("资产负债表url："+url);
		//File file=new File("S:\\外包档案管理\\私募外包业务业务档案\\B 信息披露\\C 财务报表\\管理人服务平台");
		File file=new File(url);
		List<WMap> list=new ArrayList<WMap>();
		List<WMap> filesName = filesName(file, "", list,"");
		int i = dao.insertZcfzb(filesName);
		if(i==1){
			view.set("info","数据读取成功");
		}else if(i==-1) {
			view.set("info","文件夹为空");	
		}
		else if(i==-2) {
			view.set("info","读取失败，请联系管理员");	
		}
	}




	
	public List<WMap> filesName(File file, String babh, List<WMap> list, String gjz){
		WMap dMap = new WMap();
		if(file.isDirectory()){		// 如果是目录
			File []names = file.listFiles(); // 列出全部的文件
			if(names!=null){		// 判断此目录能否列出
				for(int i=0;i<names.length;i++){
					filesName(names[i], babh, list, gjz); // 因为给的路径有可能是目录，所以，继续判断
				}
			}
		} else {
			if(file.getName().indexOf(babh) > -1 
					&& file.getName().indexOf(gjz) > -1){
				String str = file.getParent();
				str = str.replace("////", "\\");
				if(str != null){
					String []pathStr = str.split("\\\\");
					dMap.set("pName", pathStr[pathStr.length-1]);
					dMap.set("ppName", pathStr[pathStr.length-2]);
				}
    			dMap.set("fName", file.getName());
    			dMap.set("fPath", file.getPath());
    			list.add(dMap);
    			
    		}
		}
		return list;
	}
  
  
  /**
  *该方法用于 读取properties 文件   参数是properties的键，返回的是值
  */
  	public String readPro(String dm) {
 		
 		Properties pro = new Properties();
		FileInputStream fis = null;
		String nrString = "";
		try {
			fis = new FileInputStream(this.getClass().getResource("zcfzb.properties").getPath());
			pro.load(fis);
		} catch (Exception e) {
			e.printStackTrace();
			return nrString;
		} finally {
			try {
				fis.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
		nrString = pro.getProperty(dm);
 		
 		return nrString;
 	}
	
