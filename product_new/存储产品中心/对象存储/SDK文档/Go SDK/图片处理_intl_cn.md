

下面示例展示了如何在上传图片时自动实现图片处理。

图片上传完成后，COS 会存储原始图片和已处理过的图片。后续用户可以通过普通的下载请求获取处理结果。

[//]: #	".cssg-snippet-put-image-and-process"

```go
u, _ := url.Parse("https://examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com")
b := &cos.BaseURL{BucketURL: u}
c := cos.NewClient(b, &http.Client{
	Transport: &cos.AuthorizationTransport{
		SecretID:  os.Getenv("COS_SECRETID"),
		SecretKey: os.Getenv("COS_SECRETKEY"),
		Transport: &debug.DebugRequestTransport{	// 用于 Debug
			RequestHeader: true,
			RequestBody:    false,
			ResponseHeader: true,
			ResponseBody:   true,
		},
	},
})

opt := &cos.ObjectPutOptions{
	nil,
	&cos.ObjectPutHeaderOptions{
		XOptionHeader: &http.Header{},
	},
}
pic := &cos.PicOperations{
	IsPicInfo: 1,
	Rules: []cos.PicOperationsRules{
		{
             // 存储结果的目标 bucket 名称，格式为 BucketName-APPID，如果不指定，则默认保存到当前存储桶
            Bucket: "examplebucket-1250000000",
             // 处理结果的文件 Key，如以 / 开头，则存入指定文件夹中，否则，存入原图文件存储的同目录
			FileId: "example.png",
             // 处理参数，规则参见：https://cloud.tencent.com/document/product/460/6924
             // 这里为将 jpeg 转换成 png 格式
			Rule: "imageView2/format/png",
		},
	},
}
// 将处理规则转成 json 字符串, 并设置图片处理的头部
opt.XOptionHeader.Add("Pic-Operations", cos.EncodePicOperations(pic))
name := "example.jpg"
local_filename := "./example.jpg"
_, err := c.Object.PutFromFile(context.Background(), name, local_filename, opt)
```
