```dart
 import 'package:easy_contact_picker/easy_contact_picker.dart';
 
 List<Contact> _list = new List();
  final EasyContactPicker _contactPicker = new EasyContactPicker();
//获取通讯录好友
  _openAddressBook() async {
    // 申请权限
    Map<Permission, PermissionStatus> permissions =
        await [Permission.contacts].request();

    if (permissions[Permission.contacts] == PermissionStatus.granted) {
      return _getContactData();
    } else {
      return Fluttertoast.showToast(msg: "获取手机通讯录失败");
    }
  }

  _getContactData() async {
    List<Contact> list = await _contactPicker.selectContacts();
    setState(() {
      _list = list;
      print("--------${_list.length}");
    });
  }
  
  
  
  Widget _getContectItem(Contact contact) {
    return Container(
      height: 80,
      padding: EdgeInsets.only(left: 13),
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        crossAxisAlignment: CrossAxisAlignment.center,
        children: <Widget>[
          Text(contact.fullName),
          Text(
            contact.phoneNumber,
            style: TextStyle(
              color: Colors.grey,
            ),
          ),
          // Text(contact.firstLetter),
        ],
      ),
    );
  }
```

