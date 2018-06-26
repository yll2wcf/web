   release {
            //测试环境
          
            //正式环境
           buildConfigField "String", "SERVER_URL", "\"http://221.238.157.243/\""
  
          "\"apk_yunyou"+"${defaultConfig.versionCode}_${defaultConfig.versionName}"+".apk\""
            buildConfigField "String", "RELEASE_APKNAME", "\"apk_yunyou.apk\""
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'




            //这个是命名release apk的
            applicationVariants.all { variant ->
                variant.outputs.all {
                    // 输出apk名称为great_v1.0_wandoujia.apk
                    def fileName = "apk_yunyou.apk"
                    outputFileName = fileName
                  //  output.outputFile = new File(outputFile.parent, fileName)
                }
            }
