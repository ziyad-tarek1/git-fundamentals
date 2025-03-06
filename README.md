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

