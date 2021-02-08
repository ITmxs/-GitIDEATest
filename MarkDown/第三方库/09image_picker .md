







```
import 'dart:io';

import 'package:flutter/material.dart';

import 'package:image_picker/image_picker.dart';

import 'package:dio/dio.dart';

class ImagePickerPage extends StatefulWidget {
  @override
  State<StatefulWidget> createState() {
    return _ImagePickerState();
  }
}

class _ImagePickerState extends State<ImagePickerPage> {
  File _image;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text("ImagePicker"),
        ),
        body: SingleChildScrollView(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              RaisedButton(
                onPressed: _takePhoto,
                child: Text("拍照"),
              ),
              RaisedButton(
                onPressed: _openGallery,
                child: Text("选择图库照片"),
              ),
              _buildImage()
            ],
          ),
        ));
  }

  /*拍照*/
  _takePhoto() async {
    var image =
        await ImagePicker.pickImage(source: ImageSource.camera, maxWidth: 400); //从摄像头拍照

        // final pickedFile = await picker.getImage(source: ImageSource.camera);


    setState(() {
      this._image = image;
    });

    this._uploadImage(image);
  }

  /*相册*/
  _openGallery() async {
    var image =
        await ImagePicker.pickImage(source: ImageSource.gallery, );
        

    setState(() {
      this._image = image;
    });
  }

  //定义一个组件显示图片
  Widget _buildImage() {
    if (this._image == null) {
      return Text("请选择图片...");
    }
    return Image.file(this._image);
  }

  //上传图片
  _uploadImage(File _imageDir) async {
    
    //注意：dio3.x版本为了兼容web做了一些修改，上传图片的时候需要把File类型转换成String类型，具体代码如下
   
    var fileDir=_imageDir.path;       

    FormData formData = FormData.fromMap({
      "name": "zhangsna 6666666666",
      "age": 20,
      "sex": "男",    
      "file":  await MultipartFile.fromFile(fileDir, filename: "xxx.jpg")     
    });
    var response =
        await Dio().post("http://jd.itying.com/imgupload", data: formData);
    print(response);
  }
}

```

