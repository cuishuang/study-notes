---
title: 用go重构上篇代码
date: 2018-07-05 20:00:01
tags: Go
---

[上篇](http://www.dashen.tech/2018/07/04/%E4%B8%A4%E4%BD%8D%E7%9A%84-com%E5%9F%9F%E5%90%8D-%E6%9C%89%E5%A4%9A%E5%B0%91%E5%9C%A8%E4%B8%AD%E5%9B%BD%E4%BA%BA%E6%89%8B%E4%B8%AD/)使用php获取了776个两位纯字母/数字.com域名,程序为顺序执行,用时较长,在此考虑使用golang,借助其协程等特性,并发执行;

代码如下:


```go

package main

import (
	"net/http"
	"net"

	"time"
	"fmt"
	"unicode"
	"io/ioutil"
	"os"
	"strings"
)

func main() {

	sli := []string{}
	for i := 'a'; i <= 'z'; i++ {

		for j := 'a'; j <= 'z'; j++ {
			sli = append(sli, string(i)+string(j)+".com")
		}
	}

	f, err := os.Create("letter.txt")
	defer f.Close()

	if err != nil {
		panic(err)
	}

	t := time.Now()

	ch := make(chan string)
	for _, v := range sli {
		go curl(v, ch)
	}

	for i := 0; i < len(sli); i++ {
		str := <-ch

		s := strings.Split(str, "||||||")
		for _, r := range s[1] {
			if unicode.Is(unicode.Scripts["Han"], r) {

				_, err = f.Write([]byte("域名" + s[0] + "为中文网站\n"))

				fmt.Println("域名" + s[0] + "为中文网站")
				if err != nil {
					panic(err)
				}

				break
			}
		}
	}

	elapsed := time.Since(t)

	fmt.Println("消耗时间:", elapsed)
}

/**
 会自动重定向,可参考
https://my.oschina.net/fdhay/blog/690798

部分网站检测到区域为中文,会跳转到对应的中文网站,但如果在header里设置为英文后,部分原属中国的网站也可能会转为英文;在此不加设置
 */
func curl(domain string, ch chan string) string {

	c := http.Client{
		Transport: &http.Transport{
			Dial: func(network, addr string) (net.Conn, error) {
				deadline := time.Now().Add(60 * time.Second)
				c, err := net.DialTimeout(network, addr, time.Second*50)
				if err != nil {
					return nil, err
				}
				c.SetDeadline(deadline)
				return c, nil

			},
		},
	}

	res, err := c.Get("http://" + domain)

	if err != nil {
		fmt.Println("出现错误:", err)
	}

	if res != nil {
		body, _ := ioutil.ReadAll(res.Body)
		str := domain + "||||||" + string(body) //可考虑使用map优化
		ch <- str
		return str
		defer res.Body.Close()
	}

	//如果61.com无返回,尝试请求www.61.com
	if res == nil {

		res2, _ := c.Get("http://www." + domain)

		if res2 != nil {
			body, _ := ioutil.ReadAll(res2.Body)
			str := domain + "||||||" + string(body)
			ch <- str
			return str
			defer res2.Body.Close()
		}

	}

	ch <- domain + "||||||" + ""
	return ""
}


```

