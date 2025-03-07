# دليل عملي لاستخدام Git


### Git بيشتغل على 3 أماكن أساسية:  

![image](https://github.com/user-attachments/assets/b8c7d926-fc87-4d3e-bc37-7d618f081baa)

1. **الـ Working Directory** → المكان اللي بتعدل فيه الملفات.  
2. **الـ Staging Area (Index)** → المكان اللي بتحضر فيه الملفات عشان تتسجل في الكوميت الجاي.  
3. **الـ History (Commits)** → اللي هو سجل التعديلات اللي عملتها قبل كده.  

#### أوامر التعامل مع الملفات:  
- `git add <files>` → بينقل الملفات من الـ **Working Directory** للـ **Staging Area**، يعني بيجهزهم للكوميت.  
- `git commit` → بياخد نسخة من الـ **Staging Area** وبيسجلها في التاريخ (history) ككوميت جديد.  
- `git reset -- <files>` → بيشيل الملفات من الـ **Staging Area** ويرجعها زي ما كانت في آخر كوميت. لو كنت عملت `git add` بالغلط وعايز تتراجع، استخدم الأمر ده.  
- `git checkout -- <files>` → بيرجع الملفات من الـ **Staging Area** للـ **Working Directory**، يعني بيكنسل أي تغييرات محلية ويرجعك لآخر نسخة محفوظة.  
- `git reset -p` / `git checkout -p` / `git add -p` → بدل ما تحدد ملف معين، الأوامر دي بتخليك تختار أجزاء (hunks) معينة من الملف عشان تضيفها أو ترجعها.  

#### تخطي الـ Staging Area:  
- لو عايز تعمل **commit** مباشر بدون ما تضيف الملفات يدويًا للـ **Staging Area**، ممكن تستخدم:  
  - `git commit -a` → بيضيف أي ملف كان موجود في آخر كوميت بشكل أوتوماتيكي قبل ما يسجل الكوميت الجديد.  
  - `git commit <files>` → بيعمل كوميت جديد فيه آخر تعديلاتك، وكمان بينقل الملفات للـ **Staging Area** لو مش مضافة أصلاً.  
  - `git checkout HEAD -- <files>` → بيرجع الملفات من آخر كوميت للـ **Staging Area** و الـ **Working Directory**، يعني لو لخبطت وعايز ترجّع التعديلات بسرعة، ده هيكون مفيد.  

**باختصار:**  
- `git add` → يضيف الملفات للكوميت الجاي.  
- `git commit` → يسجل نسخة جديدة في الـ history.  
- `git reset` → يلغي الإضافة للـ **Staging Area**.  
- `git checkout` → يرجّع الملف لآخر حالة محفوظة.  
- `git commit -a` → يعمل كوميت بدون ما تحتاج `git add`.  

![image](https://github.com/user-attachments/assets/9c2de374-089b-4d9e-af4b-71acebd8bb59)

 

في باقي الشرح، هنعتمد على رسومات توضح تاريخ الكوميتات (Commits) والفروع (Branches) عشان تفهم Git بسهولة.  

#### تمثيل الكوميتات والفروع:  
- الكوميتات بتكون مرمّزة بـ **5 حروف** (زي: `ed489`)، ودي بتمثل الID لكل كوميت.  
- كل كوميت بيكون ليه **أب (Parent)**، يعني بيكون متصل بالكوميت اللي قبله.  
- الفروع (Branches) بتظهر باللون **البرتقالي** وبتشير لكوميت معين، يعني كل فرع هو مجرد "مؤشر" لكوميت معين.  
- الفرع الحالي بيكون متعرّف بحاجة اسمها `HEAD`، اللي بتقولك انت واقف على أي فرع حاليًا.  

#### مثال عملي:  
لو عندنا 5 كوميتات بالشكل ده:  

![image](https://github.com/user-attachments/assets/f92054ed-8fd8-4aea-9c0b-a1f095d54570)

- الكوميت **`ed489`** هو آخر كوميت على فرع `main` (اللي هو الفرع الحالي).  
- الفرع `stable` مشي مع `main` لفترة، لكنه دلوقتي واقف عند كوميت أقدم.  
- `HEAD` مربوط بفرع `main`، يعني احنا حاليًا شغالين عليه.  

بكده، أي تعديل تعمله هيكون جزء من `main`، لكن `stable` مش هيتأثر غير لو اندمجنا (merge) معاه. 🛠️

---

### **Diff - عرض الفروقات بين الكوميتات**  
ممكن تعرض الاختلافات بين الكوميتات بطرق مختلفة. الأمثلة الجاية بتوضح أهم الأوامر المستخدمة. تقدر تضيف اسم ملف معين للأمر عشان تركز الفرق عليه بس.  

![image](https://github.com/user-attachments/assets/fd4e824e-6220-47dd-a61a-7eebaed5b1d0)
 

---

### **Commit - إنشاء كوميت جديد**  
لما تعمل `git commit`، جيت بيعمل كوميت جديد باستخدام الملفات اللي في الـ **stage**، ويربطه بالكوميت الحالي كـ **Parent**، وبعدها يحرك المؤشر للفرع عليه.  

في الصورة دي، `main` كان بيشير لـ `ed489`، وبعد الكوميت الجديد (`f0cec`)، `main` بقى بيشير عليه.  

![image](https://github.com/user-attachments/assets/ee0278e9-411a-4645-87da-910b81a553ff)
 

نفس الفكرة بتحصل حتى لو الفرع الحالي هو سلف لفرع تاني. في المثال الجاي، `stable` كان سلف لـ `main`، وبعد الكوميت (`1800b`)، بقى مستقل عنه، ولو عايز تدمج التعديلات بينهم هتحتاج تعمل **merge أو rebase**.  

![image](https://github.com/user-attachments/assets/762d6d16-8262-4d72-ae61-70ca829ffee8)
  

لو عملت غلطة في الكوميت، ممكن تصلحها بسهولة باستخدام:  
```bash
git commit --amend
```
الأمر ده بيعمل كوميت جديد بنفس الـ **parent** للكوميت الحالي، والقديم بيتحذف لو مفيش حاجة بتشاور عليه.  

![image](https://github.com/user-attachments/assets/02fbabbb-8862-48ec-a3b4-6ff1b0fecae6)
 

---

### **Checkout - استرجاع الملفات أو التنقل بين الفروع**  
الأمر `git checkout` بيستخدم عشان **ترجع ملف من تاريخ معين** أو **تنقل بين الفروع**.  

لو استخدمت اسم ملف مع الأمر، هيرجع لك الإصدار القديم من الملف للـ **stage** و الـ **working directory**.  
مثال:  
```bash
git checkout HEAD~ foo.c
```
الأمر ده هيرجع الملف `foo.c` من الكوميت اللي قبله (`HEAD~`) ويحطه في الـ **working directory** ويعمله **stage**، لكن الفرع نفسه مش هيتغير.  

![image](https://github.com/user-attachments/assets/e8a387d9-8aeb-49cb-83f1-8b225ad4d79c)

---
### **التنقل بين الفروع باستخدام Checkout**  
لو استخدمت `git checkout` بدون تحديد اسم ملف، وكان المرجع فرع محلي، **HEAD** هيتحرك للفرع ده، وهيتم تحديث **stage** و **working directory** عشان يطابقوا محتوى الكوميت الجديد.  

- أي ملف موجود في الكوميت الجديد هيتنسخ للـ **working directory**  
- أي ملف كان موجود في الكوميت القديم ومش موجود في الجديد هيتحذف  
- أي ملف مش موجود في أي من الكوميتين هيتم تجاهله  

![image](https://github.com/user-attachments/assets/2bb16ebd-2851-4bdd-85e3-c5b39552ba1d)


---

### **Detached HEAD - العمل خارج الفروع العادية**  
لو استخدمت `git checkout` بدون تحديد اسم ملف، وكان المرجع **مش فرع محلي** (زي **tag**، **remote branch**، **SHA-1 ID**، أو حاجة زي `main~3`)، هتدخل في **Detached HEAD**.  

ده مفيد لو عايز تتنقل في الـ **history**. مثلا، لو عايز ترجع لإصدار قديم من جيت (`v1.6.6.1`):  
```bash
git checkout v1.6.6.1
```
بعد ما تخلص، تقدر ترجع لفرع تاني عادي:  
```bash
git checkout main
```
لكن لو عملت **commit** أثناء وجودك في **Detached HEAD**، الكوميت هيبقى موجود لكن مش مربوط بأي فرع.  

![image](https://github.com/user-attachments/assets/58534d26-0edf-4c0b-8939-3107e27e83a3)


---

### **Committing with a Detached HEAD - الكوميت بدون فرع**  
في وضع **Detached HEAD**، لو عملت `git commit`، الكوميت هيتم إنشاؤه عادي لكن مش هيتم تحديث أي فرع باسم محدد، وده معناه إن الكوميت ممكن يضيع لو انتقلت لحاجة تانية.  

![image](https://github.com/user-attachments/assets/52ba0c87-6926-414a-af11-59207ea82fd3)


لو عايز تحفظ الحالة دي، تقدر تعمل فرع جديد باستخدام:  
```bash
git checkout -b new-branch
```
كده الكوميت هيبقى محفوظ ومش هيضيع.  

![image](https://github.com/user-attachments/assets/c56a9a14-6a35-4396-bf84-70860b9a916e)

![image](https://github.com/user-attachments/assets/d6c1fde6-6480-4086-9e17-dc5f8ee386eb)


---

### **Reset - إعادة تعيين الفرع**  
الأمر `git reset` بيستخدم عشان تحرك الفرع الحالي لكوميت تاني، وكمان ممكن يحدث **stage** و **working directory** حسب الحاجة.  

#### **أوضاع `git reset`**  
- لو كتبت `git reset <commit>` بدون أسماء ملفات، الفرع الحالي هيتحرك للكوميت ده، وهيتم تحديث **stage** عشان يطابقه.  
- لو أضفت `--hard`، **working directory** كمان هيتحدث وهيتم حذف أي تعديلات غير محفوظه.  
- لو أضفت `--soft`، **لا الـ stage ولا الـ working directory** هيتغيروا، بس الفرع هيتحرك للكوميت الجديد.  

![image](https://github.com/user-attachments/assets/f4398112-c775-4e7e-ac8e-cc6378dc6256)


لو ما حددتش كوميت، `git reset` بيشتغل على `HEAD`، وفي الحالة دي الفرع مش هيتحرك، لكن **stage (وكمان الـ working directory لو استخدمت --hard) هيرجعوا لحالة آخر كوميت**.  

![image](https://github.com/user-attachments/assets/4d86cc70-37a8-442a-bd3c-8c04cafddb69)

#### **إعادة تعيين ملفات معينة**  
لو استخدمت `git reset` مع **اسم ملف**، هيرجع الملف ده بس للـ **stage**، لكن مش هيلمس **working directory**. (وده نفس تأثير `git checkout` بس مع الفرق إن **working directory مش بيتأثر**).  

![image](https://github.com/user-attachments/assets/fd3a618b-d9b5-4681-b301-e3c791f8ae93)

---
### **Merge - دمج الفروع**  
`git merge` بيستخدم لدمج التعديلات من فرع آخر في الفرع الحالي، وبيتم إنشاء **كوميت جديد** يدمج التعديلات دي.  

#### **أنواع الـ Merge**  
1. **Fast-Forward Merge**:  
   - لو كان الفرع الآخر **امتداد مباشر** للفرع الحالي، الجيت بس بينقل المؤشر للفرع الجديد بدون إنشاء كوميت جديد.  
   - مثال:  
     ```bash
     git checkout main
     git merge feature-branch
     ```
![image](https://github.com/user-attachments/assets/f84f3890-ad09-4c06-b093-c2cd42c6af3a)

2. **Recursive Merge (ثلاثي الأطراف)**:  
   - لو الفرعين حصل عليهم تعديلات منفصلة، الجيت بيحدد **السلف المشترك** ويستخدمه كمرجع للدمج.  
   - بعد حل أي تعارضات، بيتم **إنشاء كوميت جديد** يحتوي على التعديلات المدمجة.  
![image](https://github.com/user-attachments/assets/363b8b57-1b13-4a1a-8c63-cc146f518c91)

---

### **Cherry-Pick - اختيار كوميت معين**  
`git cherry-pick` بيستخدم لنسخ **كوميت معين** من فرع آخر إلى الفرع الحالي، مع الحفاظ على نفس الرسالة والتعديلات.  

#### **مثال: نسخ كوميت معين للفرع الحالي**  
```bash
git checkout main
git cherry-pick <commit-hash>
```
- ده مفيد لو عايز تاخد **ميزة أو إصلاح** من فرع تاني بدون دمج كامل.  

![image](https://github.com/user-attachments/assets/31152a22-1201-4362-b952-d152c6c4a2fc)

---

### **Rebase - إعادة ترتيب الكوميتات**  
بديل للـ `merge`، لكنه بيعيد تشغيل كل الكوميتات من الفرع الحالي فوق فرع آخر، **لإبقاء التاريخ خطي** بدون تشعبات.  

#### **مثال: إعادة تشغيل الكوميتات من `feature-branch` على `main`**  
```bash
git checkout feature-branch
git rebase main
```
- ده بيجعل كل تعديلات `feature-branch` تظهر **وكأنها تمت بعد آخر تحديث للـ main**.  
![image](https://github.com/user-attachments/assets/714cea3d-03ef-48ab-9802-56f77b405de9)

#### **Rebase --onto**  
لو عايز تنقل مجموعة معينة من الكوميتات فقط، استخدم `--onto`:  
```bash
git rebase --onto main 169a6 feature-branch
```
- هنا بيتم **نقل الكوميتات من بعد 169a6** إلى `main`.  
![image](https://github.com/user-attachments/assets/014cc6d3-3e51-4456-96a6-5a5e2cf6b64c)

#### **Interactive Rebase - تعديل الكوميتات أثناء إعادة ترتيبها**  
```bash
git rebase -i HEAD~3
```
- يسمح لك بـ **حذف، إعادة ترتيب، دمج، أو تعديل** الكوميتات أثناء الـ rebase.

---
---

## Git Branches - إدارة الفروع
الفروع في Git هي مجرد مؤشرات تشير إلى كوميت معين. 

- **إنشاء فرع جديد**:
  ```bash
  git branch sarah
  ```
- **التنقل بين الفروع**:
  ```bash
  git checkout sarah
  ```
- **إنشاء فرع جديد والانتقال إليه**:
  ```bash
  git checkout -b max
  ```
- **حذف فرع**:
  ```bash
  git branch -d max
  ```
- **عرض جميع الفروع**:
  ```bash
  git branch
  ```

---

## Merging Branches - دمج الفروع

- **Fast-forward merge**: يحدث عندما لا يحتوي الفرع الحالي على أي تعديلات إضافية مقارنة بالفرع المراد دمجه، فيتم تحريك المؤشر مباشرة بدون إنشاء كوميت جديد.
- **No-fast-forward merge**: في حال وجود تعديلات على الفرع الحالي، يتم إنشاء كوميت دمج جديد.

---

## Initialize Remote Repositories - إضافة مستودع عن بعد
لإضافة مستودع عن بعد وتحديده باسم `origin`:
```bash
git remote add origin <remote_connection_string>  # origin is alias for <remote_connection_string> and is called conection stream
```
للتحقق من الريموت:
```bash
git remote -v
```

---

## Pushing to Remote Repositories - رفع التعديلات إلى الريبو البعيد
```bash
git push origin master
```

---

## Cloning Remote Repositories - استنساخ مستودع بعيد
```bash
git clone <ssh_link>
```

---

## Pull Requests - طلبات الدمج
### استخدام الـ GUI:
- **سحب التعديلات إلى نفس الفرع** عبر واجهة GitHub أو GitLab.
- **فتح PR من فرع فيتشر إلى الفرع الرئيسي** من خلال زر `New Pull Request`.

### استخدام الـ CLI:
```bash
git fetch origin main
git merge origin/main
```
أو لفتح PR:
```bash
git checkout -b feature-branch
git push origin feature-branch
```
ثم تفتح PR من GitHub/GitLab.

---

## Rebasing - إعادة ترتيب الكوميتات
### متى تستخدم الـ Rebase؟
- لو عندك فرع متأخر عن `main` وعايز تدمج التعديلات بدون كوميت دمج إضافي.
- عشان تحافظ على الـ history نظيف بدل ما يكون فيه "Merge commit" لكل تعديل.

### كيف تعمل Rebase؟
```bash
git checkout feature-branch
git rebase main
```
لو حصل تعارض (`conflict`)، هتحتاج تحل التعارضات وتكمل:
```bash
git rebase --continue
```

### Interactive Rebasing - تعديل عدة كوميتات دفعة واحدة
```bash
git rebase -i HEAD~4
```
هتظهر لك قائمة فيها كل الكوميتات، تقدر تستخدم:
- `pick` للإبقاء على الكوميت كما هو.
- `squash` لدمج كوميتات متعددة في واحد.
- `edit` لتعديل كوميت معين.

---

## Cherry-Picking - اختيار كوميت معين
لو عندك فرع فيه تعديل معين عايز تاخده لفرع تاني بدون باقي التعديلات:
1. **افتح الفرع اللي عايز تنقل إليه التعديل**
   ```bash
   git checkout main
   ```
2. **خذ الكوميت من الفرع الآخر**
   ```bash
   git cherry-pick <commit_hash>
   ```

مثال: لو عندك `feature-branch` وفيه تعديل حلو، وعايزه على `main` بدون باقي التعديلات:
```bash
git checkout main
git cherry-pick <commit_hash>
```

---

## Resetting and Reverting - التراجع عن التعديلات
### `git reset` vs `git revert`
- **`git reset`** يغير التاريخ بدون إنشاء كوميت جديد.
- **`git revert`** ينشئ كوميت جديد يزيل التعديلات السابقة.

### Reset:
- **التراجع بدون فقدان الملفات:**
  ```bash
  git reset --soft HEAD~1
  ```
- **التراجع وحذف التعديلات نهائيًا:**
  ```bash
  git reset --hard HEAD~1
  ```

### Revert:
- لو عايز تلغي كوميت معين مع الاحتفاظ بباقي التعديلات:
  ```bash
  git revert <commit_hash>
  ```

---

## Stashing - تخزين التعديلات مؤقتًا
لو عندك تعديلات في `working directory` وعايز تخزنها بدون ما تعمل كوميت:
```bash
git stash
```
لاسترجاع التعديلات:
```bash
git stash pop
```
لعرض الـ stash الموجود:
```bash
git stash list
```
لحذف stash معين:
```bash
git stash drop <stash_id>
```

