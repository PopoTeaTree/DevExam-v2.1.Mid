# DevExam-v2.1.Mid
จากข้อมูลวางแผนการผลิตของโรงงานเพื่อกระจายสิ้นค้าไปยังร้านค้า 
ข้อมูลที่ได้รับมาจะมี 2 ตาราง
1. FactoryPlan ข้อมูลแผนการผลิตของโรงงาน
2. StorePlan ข้อมูลแผนการขายของร้านค้า
โรงงานได้รับแผนการผลิต(FactoryPlan.PlanQty) มาแล้ว และเกิดการผลิตขึ้นจริงแล้ว (FactoryPlan.ActualQty) โดยอาจจะผลิตได้มากกว่า หรือน้อยกว่าแผน
เมื่อทราบจำนวนที่ผลิตได้แล้ว โรงงานต้องส่งสินค้าไปยังร้านค้าต่างๆ ที่กำหนดไว้ใน StorePlan และใน 1 โรงงานจะผลิตสินค้าให้กับร้านค้ามากกว่า 1 ร้าน 
การกระจายสินค้าจะเป็นไปตามแผนการขายของร้านค้า (StorePlan.PlanQty) ว่าต้องได้รับจำนวนกี่ชิ้น แต่ว่าโรงงานไม่สามารถผลิตได้ตามแผนที่กำหนดไว้
อยากทราบว่าการกระจายสินค้าไปร้านค้าต่างๆนั้น ร้านค้าต้องได้รับ (StorePlan.ActualQty) จำนวนกี่ชิ้น

ให้เขียนโปรแกรมหาว่า หน้าร้านจะต้องได้รับสินค้าจำนวนจำนวนเท่าไหร่ โดยใช้การ weight จากแผนกาผลิตและจำนวนที่ผลิตได้จริง 
โดยจำนวนสินค้าต้องกระจายหน้าร้านครบทุกชิ้นไม่เหลือค้างที่โรงงานแม้ว่าจะผลิตได้มากกว่าหรือน้อยกว่าแผน
สามารถจำลองข้อมูลใน Array, List, Database หรือเครื่องมืออื่นๆ ได้
## Project environment
1. Lanugage : Python (3.8)
2. Tool & run : Jupyter notebook
3. Library : Pandas, openpyxl, numpy (Avable to install in Juptyter notebook "!pip install LIBRARY_NAME")
4. Project file: NTTDevExam-v2.1.Mid-Thitiporn.ipynb
5. Do not forget to set path for input document and ouput document (.xlsx)
P.S. Install Python 3.8 and jupyter notebook first (https://jupyter.org/install)
## Description
### กระบวนการทำงานคร่าวๆ
1. import data .xml to Data frame
2. เก็บ Factory ID ของ Foctory ที่ผลิต ActualQty มากกว่า PlanQty ใน array และ เก็บ Factory ID ของ Foctory ที่ผลิต ActualQty น้อยกว่า PlanQty ใน arrayเช่นกัน
3. Update ActualQty ของ Factory ที่ ActualQty มากกว่า PlanQty และ Update ว่าเหลือเท่าไหร่
5. Init demand weight(โดยการ หารPlanQty และ ActualQty)ของแต่ละ factory แล้วเรียงจากมากไปน้อย เพื่อลำดับความต้องการ
6. Update ActualQty ของ Store โดยใช้ demand wight และ PlanQty เทียบบรรญีติไตรยางค์ แล้วปัดเศษขึ้น
7. Update ActualQty ของ Store อีกครั้ง เพราะ เมื่อปัดเศษขึ้น ทำให้ ActualQty ของ Foctory ไม่สอดคล้องกับใน product โดย Update ActualQty ของ Store 
ที่เป็น Foctory ที่ผลิต ActualQty มากกว่า PlanQty
8. Init ความสมบูณ์ ของ order ว่าได้รับของกี่เปอร์เซ็นแล้ว จาก plan
9. Check ว่า ActualQty ของ Foctory และ ActualQty ของ Store ว่ามากกว่า น้อยกว่า เท่ากับ
- มากกว่า : sort ระดับความสัมคัญของ dataframe จาก demand weigh และ เปอร์เซ็นความ complete แล้ว update dataframe
- น้อยกว่า : sort ระดับความสัมคัญของ dataframe จาก demand weigh และ เปอร์เซ็นความ complete แล้ว ลบ ActualQty
- เท่ากับ
10. Drop column ที่ไม่เกี่ยวข้อง
11. Export to .xml
### P.S. 
1. ActualQty ของ Foctory ที่ผลิต ActualQty มากกว่า PlanQty จะเป็นของที่เหลือ จากการ Update การส่ง
2. PlanQty ของ Foctory ที่เป็น "0" เกิดจากการแบ่งของ Foctory ที่ผลิต ActualQty มากกว่า PlanQty
