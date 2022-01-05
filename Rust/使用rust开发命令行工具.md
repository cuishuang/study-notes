---
title: 使用rust开发命令行工具
date: 2016-09-12 21:55:05
tags: Rust
---



生成二进制文件，将其扔到环境变量的path下即可~


<br>

### [用rust打造实时天气命令行工具](https://www.bilibili.com/video/BV1eL411b7EL)

<br>

#### 找到合适的API

<br>

使用[该api](https://openweathermap.org/api)


<img src="使用rust开发命令行工具/0.png" width = 80% height = 100% />


<img src="使用rust开发命令行工具/1.png" width = 80% height = 100% />



如请求 `api.openweathermap.org/data/2.5/weather?q=Beijing&appid=your_key`:


```json
{
	"coord": {
		"lon": 116.3972,
		"lat": 39.9075
	},
	"weather": [{
		"id": 803,
		"main": "Clouds",
		"description": "broken clouds",
		"icon": "04d"
	}],
	"base": "stations",
	"main": {
		"temp": 293.35,
		"feels_like": 292.34,
		"temp_min": 291.09,
		"temp_max": 294.13,
		"pressure": 1026,
		"humidity": 35,
		"sea_level": 1026,
		"grnd_level": 1020
	},
	"visibility": 10000,
	"wind": {
		"speed": 4.86,
		"deg": 344,
		"gust": 7.43
	},
	"clouds": {
		"all": 73
	},
	"dt": 1634262993,
	"sys": {
		"type": 2,
		"id": 2021025,
		"country": "CN",
		"sunrise": 1634250256,
		"sunset": 1634290552
	},
	"timezone": 28800,
	"id": 1816670,
	"name": "Beijing",
	"cod": 200
}
```

<br>



#### 初始化项目&coding

<br>

使用`cargo new rust_weather` 初始化一个项目。



对于*cargo.toml*文件：

```toml
[package]
name = "rust_weather"
version = "0.1.0"
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
structopt = "0.3.21"
exitfailure = "0.5.1"
serde = "1.0.114"
serde_json = "1.0.56"
serde_derive = "1.0.114"
reqwest = { version = "0.11", features = ["json"] }
tokio = { version = "1", features = ["full"] }
```

<br>

对于*src/main.rs*文件：


```rust
use exitfailure::ExitFailure;
use reqwest::Url;
use serde_derive::{Deserialize, Serialize};
use structopt::StructOpt;

#[derive(Serialize,Deserialize,Debug)]
struct W {
    coord: Coord,
    weather: Weather,
    base: String,
    main: Main,
}

impl W {
    async fn get(city: &String) -> Result<Self, ExitFailure> {
        let url = format!("https://api.openweathermap.org/data/2.5/weather?q={}&appid=40452068d845180226c3f289341974b7", city);
        // 转换为url
        let url = Url::parse(&*url)?;
        let resp = reqwest::get(url).await?.json::<W>().await?;
        Ok(resp)
    }
}

#[derive(Serialize,Deserialize,Debug)]
struct Coord {
    lon: f64,
    lat: f64,
}

#[derive(Serialize,Deserialize,Debug)]
struct Weather {
    details: Details,
}

#[derive(Serialize,Deserialize,Debug)]
struct Details {
    id: i32,
    main: String,
    description: String,
    icon: String,
}

#[derive(Serialize,Deserialize,Debug)]
struct Main {
    temp: f64,
    feels_like: f64,
    temp_min: f64,
    temp_max: f64,
    pressure: i32,
    humidity: i32,

}


#[derive(StructOpt)]
struct Input {
    city: String
}

#[tokio::main]
async fn main() -> Result<(), ExitFailure> {
    let input = Input::from_args();
    //println!("{}", input.city);


    let resp = W::get(&input.city).await?;

    println!("{} \n 天气: {} \n 当前温度: {} \n 最高温度: {} \n 最低温度: {} \n 湿度: {}", input.city, resp.weather.details.main, resp.main.temp, resp.main.temp_max, resp.main.temp_min, resp.main.humidity);

    //println!("Hello, world!");
    Ok(())
}

```

<br>

使用`cargo run Beijing`进行调试

直到能够准确输出预订结果，如下：

```rust
➜  rust_weather git:(master) ✗ cargo run Beijing
    Finished dev [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/rust_weather Beijing`
Beijing 
 天气: Clouds 
 当前温度: 293.35 
 最高温度: 294.13 
 最低温度: 291.09 
 湿度: 35
```


<br>



#### 将二进制文件移动到系统PATH路径下

<br>



此时**target/debug/rust_weather**即想要的二进制文件，可将其复制到任意一个系统PATH路径下


 `echo $PATH`

 ```sh
/opt/homebrew/opt/node@12/bin:/Users/fliter/.nvm/versions/node/v16.9.0/bin:/usr/local/Cellar/mysql@5.7/5.7.28/bin:/opt/homebrew/bin:/opt/homebrew/sbin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/go/bin:/Users/fliter/.cargo/bin:/usr/local/go/bin:/Users/fliter/go/bin:/Users/fliter/Downloads/:/bin:/usr/local/MongoDB/bin:/usr/local/Cellar/ffmpeg/4.3.1/bin:/Users/fliter/.cargo/bin
```


还可以重命名，如改为`weather`,复制到*usr/local/bin*下，而后*source .zshrc*


<br>


在任意命令行窗口下，执行 `weather Binzhou`:

```sh
Binzhou 
 天气: Rain 
 当前温度: 291.63 
 最高温度: 291.63 
 最低温度: 291.63 
 湿度: 67
```



<br>

参考自[原子之音](https://www.bilibili.com/video/BV1eL411b7EL)