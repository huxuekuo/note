# SpringBoot Feign Current request is not a multipart request



## 问题描述



来句白话: 当前请求不是长传文件的请求,  当前请求的`Content-Type` 不是`multipart/form-data` 在是我使用的是feign,偶买噶怎么办, 



## 看代码

```java
  @ApiOperation(value = "多文件上传", notes = "多文件上传")
    @PostMapping(value = "/feigne/cos/uploadFiles")
    @Headers({"Content-Type: multipart/form-data"})
    public List<Qd> uploadFiles(@ApiParam(name = "files", value = "文件") @RequestParam("files") MultipartFile[] files,
                                @ApiParam(name = "fileType", value = "文件类型") @RequestParam("fileType") String fileType);
```



`@Headers({"Content-Type: multipart/form-data"})` 加入@Headers注解,完美解决, 在使用feign时修改headers信息了, 偶买噶赞他