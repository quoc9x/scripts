# Cài đặt trên Linux
```
wget https://dlcdn.apache.org//jmeter/binaries/apache-jmeter-5.6.3.tgz
tar -xvf apache-jmeter-5.6.3.tgz
```

# Run
```
cd apache-jmeter-5.6.3/bin/
./jmeter.sh -n -t ~/demo.jmx -j tmp1.log -l tmp2.xml
```
- demo.jmx - file test plan (sửa thông tin trong file này để thay đổi kịch bản test)
- tmp1.log - đây là file chứa log
- tmp2.xml - file result của jmeter (call http request 200, 404, 403 sẽ show hết ở đây)
- -n - là chạy với mode NONE GUI