# 基于golang语言web框架gin搭建应用-02配置文件\(yml\)

新建`common`文件夹，配置文件采用的`yml`文件，项目中可能用到数据库\(mysql,postgres,redis\)，消息队列等，不同的配置可能名称相同， 故采用了不同的struct来获得不同的配置信息。

## 配置文件

在`common`文件夹下创建`config.go`文件，输入下面内容,其中tag中的yml对应的值和`config.yml`中的一一对应

    //server config will used in file variables.go
    type configModel struct {
        Server              *serverModel       `yaml:"server"`

    }
    //serverModel get server information from config.yml
    type serverModel struct {
        Mode                 string        `yaml:"mode"`                    // run mode
        Host                 string        `yaml:"host"`                    // server host
        Port                 string        `yaml:"port"`                    // server port
        EnableHttps          bool          `yaml:"enable_https"`            // enable https
        CertFile             string        `yaml:"cert_file"`               // cert file path
        KeyFile              string        `yaml:"key_file"`                // key file path
        JwtPubKeyPath        string        `yaml:"jwt_public_key_path"`     // jwt public key path
        JwtPriKeyPath        string        `yaml:"jwt_private_key_path"`    // jwt private key path
        TokenExpireSecond    time.Duration `yaml:"token_expire_second"`     // token expire second
        SystemStaticFilePath string        `yaml:"system_static_file_path"` // system static file path
    }

在`common`文件夹下创建`variables.go`文件，引入`config.go`文件中用到的struct, `ConfigInfo`为获得的所有配置信息的根，`variables.go`中其他变量\(全局\)都将冲`ConfigInfo`中获得信息，为了便于在别的地方使用，故将他们单独拿出来。由于除了配置文件地址，应用后面还需要使用到配置文件位置，故增加`WorkSpace`来保存配置文件位置

```
var (
    WorkSpace  string       // config
    ServerInfo              *serverModel       // server config information
    ConfigInfo *configModel // all server config information
)
```

### 配置config.yml

```
# config information

# server config information
server:
  mode: debug # debug,release
  host: 0.0.0.0
  port: 8000
  token_expire_second: 360000
  enable_https: false  # 后台开启https
  cert_file: conf/https/cert.pem  # https对应的私钥
  key_file: conf/https/key.pem # https对应的公钥
  jwt_public_key_path: conf/jwt/tm.rsa.pub
  jwt_private_key_path: conf/jwt/tm.rsa
  system_static_file_path: system_statics
```

## 配置文件加载

创建`system`文件夹，增加`config.go`文件,`LoadConfigInformation`函数用于获得配置文件的信息,该函数默认会从当前项目的`conf`文件夹加载`config.yml`，获得其中的信息

```
/LoadConfigInformation load config information for application
func LoadConfigInformation(configPath string) (err error) {
    var (
        filePath string
        wr       string
    )

    if configPath == "" {
        wr, _ = os.Getwd()
        wr = path.Join(wr, "conf")

    } else {
        wr = configPath
    }
    common.WorkSpace = wr
    filePath = path.Join(common.WorkSpace, "config.yml")
    configData, err := ioutil.ReadFile(filePath)
    if err != nil {
        fmt.Printf(" config file read failed: %s", err)
        os.Exit(-1)

    }
    err = yaml.Unmarshal(configData, &common.ConfigInfo)
    if err != nil {
        fmt.Printf(" config parse failed: %s", err)

        os.Exit(-1)
    }
    // server information
    common.ServerInfo = common.ConfigInfo.Server
    return nil
}
```

修改`main.go`的代码，在`main`函数中调用`LoadConfigInformation` ,

```
func main() {
    var err error
    fPath, _ := os.Getwd()
    fPath = path.Join(fPath, "conf")
    configPath := flag.String("c", fPath, "config file path")
    flag.Parse()
    err = system.LoadConfigInformation(*configPath)
    fmt.Printf("%+v\n",common.ConfigInfo.Server)
    if err != nil {
        return
    }
}
```

运行，查看打印结果，测试通过之后删除`fmt.Printf("%+v\n",common.ConfigInfo.Server)`和对应的包

```
&{Mode:debug Host:0.0.0.0 Port:8000 EnableHttps:false CertFile:conf/https/cert.pem KeyFile:conf/https/key.pem JwtPubKeyPath:conf/jwt/tm.rsa.pub JwtPriKeyPath:conf/jwt/tm.rsa TokenExpireSecond:360µs SystemStaticFilePath:system_statics}
```



