# Java stream

Stream เป็นเครื่องมือในการจัดการข้อมูลซึ่งมีประสิทธิภาพสูง สามารถกรอง(filter), เรียงลำดับ โดยจะเขียน code ได้สั้นลงมาก 
จาก code ด้านล่างซึ่งเป็นการหาค่าผมรวมของ arr ซึ่งจากหาค่าที่มากกว่า 5 โดย code ด้านซ้ายและด้านขวาทำงานได้เหมือนกัน แต่ code ทางด้านขวาจะสั้นกว่าทางด้านซ้ายถึง 6 บรรทัด
```
int arr[] = {1,2,3,6,7};         int arr[] = {1,2,3,6,7};
int result = 0;                  int result = IntStream.of(arr).filter(e -> e > 5).sum();
for(i=0;i<arr.length;i++){
    int arrValue = arr[i];
    if(arrValue > 5){
        result = result+arr[i];
    }
}
```

Stream เริ่มมีครั้งแรกใน Java 8 ผู้เขียนได้อ่านบทความจาก [dzone](www.dzone.com) เรื่อง [Become a Master of Java Streams](https://dzone.com/articles/become-a-master-of-java-streams-part-1-creating-st)
เลยอยากเขียนเป็นของเราเอง(อยากเขียนมานานมาก) ซึ่งไม่ได้เป็นการแปลแต่อย่างใด การเขียนหรือการสอนบุคคลอื่นจะ
ทำให้ความรู้ของเราเรื่องนั้นมีความเข้าใจยิ่งขึ้น

stream เป็นการจัดการข้อมูลซึ่งไหลมา stream แบ่งเป็น 3 ส่วน  
![Dzone stream image](https://1.bp.blogspot.com/-XEU2WqWiI4g/XZc3e0v8djI/AAAAAAAAAhg/WTdc1dqVwiUAmizN-abuvSNRWuYSy_UrQCEwYBhgL/s1600/Ska%25CC%2588rmavbild%2B2019-10-03%2Bkl.%2B09.42.17.png)

1. Stream source เป็นแหล่งที่มาข้อมูลใน stream
2. Intermediate operations เป็นการจัดการข้อมูลเช่น filter, sorted ข้อมูล
3. Terminal operations เป็นการดำเนินการสุดท้าย

code ด้านล่างเป็นตัวอย่างการใช้งาน stream โดยจะเป็นการสร้าง stream และกรองข้อมูลที่มากกว่า 5 จากนั้นเรียงลำดับข้อมูล
```$java
List lst = Stream.of(11, 1, 10, 21, 5, 9, 2, 3, 10, 21)
                .filter(e -> e > 5)
                .sorted()
                .collect(Collectors.toList());
System.out.println(lst);
```
จาก code เมื่อนำไป run จะได้ผลลัพธ์ดังนี้
```$java
[9, 10, 10, 11, 21, 21]
```

สำหรับ stream จะขอพูดใน 2 เรื่องคือ

* การสร้าง stream
* การใช้งาน intermediate operations

## การสร้าง stream

การสร้าง stream สามารถทำได้หลายวิธีเช่น สร้างจาก array, สร้างจาก collection, ดึงข้อมูลจาก file เป็นต้น
### การสร้าง stream จาก array 
จะเป็นดัง code ด้านล่าง เห็นได้ว่า s1 และ s2 เป็น stream ที่สร้างจาก array
```$java
Stream s1 = Stream.of(new int[]{1,2,3,4});
Stream s2 = Stream.of(1,2,3,4);
```

### การสร้าง stream จาก collection
เมื่อมี collection สามารถเปลี่ยนเป็น stream ได้ดัง code ด้านล่าง โดย s3 เป็น stream ของ List และ s4 เป็น stream ของ Set
```$java
List lst = List.of("a","b","c","d","e");
Stream s3 = lst.stream();

Set s = Set.of(1, 2, 3);
Stream s4 = s.stream();
```

### การสร้าง stream จาก Stream source
โดย Stream source เป็นเครื่องมือของจาวา 
```
Stream
IntStream
LongStream
DoubleStream
```
การสร้าง stream ด้วย  Stream source แบบนี้จะทำให้จาวาสามารถสร้าง array ในลักษณะต่างได้อย่างง่ายดายเช่น array ของเลขสุ่มขนาด 10 ตัว, array ของเลข 1 ถึง 9 เป็นต้น
การสร้าง stream ด้วย Stream source สามารถทำได้ดัง code ด้านล่าง
```
IntStream s5 = IntStream.range(0, 10);
LongStream s6 = LongStream.rangeClosed(0, 10);
DoubleStream s7 = new Random().doubles(10, 10, 100); //สร้างเลขสุ่ม 10 ตัว มีค่า 10 ถึง 100
```
การเปลี่ยน Stream source เป็น array สามารถทำได้โดยใช้ toArray()
```
int arr [] = IntStream.rangeClosed(1, 10).toArray();
```

## การใช้งาน intermediate operations
เป็นการจัดการข้อมูลโดยมีคำสั่งดังนี้ filter, limit, skip, distinct, sorted, map, flatmap ผู้ใช้สามารถใช้มากกว่า 1 คำสั่งต่อกันได้
```
Stream s8 = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
                .filter(e -> e > 2)
                .limit(5);
System.out.println(Arrays.toString(s8.toArray()));                
```
s8 จะเป็น stream ซึ่งจำกัดแค่ 5 จำนวน โดยค่าจะมากกว่า 2
```
[3, 4, 5, 6, 7]
```

* filter 

เป็นการกรองข้อมูล จาก code ด้านล่างจะกรองค่าซึ่งมากกว่า 3
```
Stream s8 = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
                .filter(e -> e > 3);
```
ผลลัพธ์
```
[4, 5, 6, 7, 8, 9, 10]
```
* limit 

เป็นการจำกัดขนาดของข้อมูล
```
Stream s8 = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
                .limit(2);
```
ผลลัพธ์
```
[1, 2]
```

* skip 

เป็นการข้ามข้อมูล
```
Stream s8 = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
                .skip(5);
```
ผลลัพธ์
```
[6, 7, 8, 9, 10]
```

* distinct

เป็นการเอาเฉพาะข้อมูลที่ไม่ซ้ำกัน
```
Stream s8 = Stream.of(1, 1, 1, 2, 3)
                .distinct();
```
ผลลัพธ์
```
[1, 2, 3]
```

* sort 

เป็นการเรียงลำดับของข้อมูล
```
Stream s8 = Stream.of(2, 3, 1, 6, 7, 4)
                .sorted();
```
ผลลัพธ์
```
[1, 2, 3, 4, 6, 7]
```
* map

เป็นการเปลี่ยนข้อมูล โดย code ด้านล่างเป็นการคูณ 10 ให้กับข้อมุลทุกตัว
```
Stream s8 = Stream.of(1, 2, 3, 4, 5, 6)
                .map(e -> e * 10);
```
ผลลัพธ์
```
[10, 20, 30, 40, 50, 60]
```
