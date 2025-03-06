# دليل عملي لاستخدام Git

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

