# 🎮 Hướng dẫn GitHub cho dự án Unity

> Tài liệu này hướng dẫn chi tiết cách thiết lập và quản lý dự án Unity trên GitHub,
> bao gồm cách tránh các lỗi phổ biến và xử lý khi có sự cố.

---

## 📋 Mục lục

1. [Thiết lập dự án mới](#1-thiết-lập-dự-án-mới)
2. [Cấu hình .gitignore](#2-cấu-hình-gitignore)
3. [Git LFS cho file lớn](#3-git-lfs-cho-file-lớn)
4. [Workflow hàng ngày](#4-workflow-hàng-ngày)
5. [Xử lý lỗi phổ biến](#5-xử-lý-lỗi-phổ-biến)
6. [Làm việc nhóm](#6-làm-việc-nhóm)
7. [Khôi phục dự án](#7-khôi-phục-dự-án)

---

## 1. Thiết lập dự án mới

### Bước 1 — Tạo repository trên GitHub

1. Vào **github.com** → **New repository**
2. Đặt tên repo (VD: `CardGame`)
3. Chọn **Private** hoặc **Public**
4. ✅ Tích **Add a .gitignore** → chọn template **Unity**
5. Nhấn **Create repository**

### Bước 2 — Clone về máy

```bash
git clone https://github.com/<username>/<repo>.git
cd <repo>
```

Sau đó copy toàn bộ project Unity vào thư mục vừa clone.

### Bước 3 — Hoặc liên kết project Unity có sẵn

```bash
# Trong thư mục project Unity
git init
git remote add origin https://github.com/<username>/<repo>.git

# Tạo .gitignore TRƯỚC KHI add file
# (xem mục 2 bên dưới)

git add .
git commit -m "Initial commit"
git push -u origin main
```

> ⚠️ **QUAN TRỌNG:** Phải tạo `.gitignore` trước bước `git add .`
> Nếu không, Unity sẽ commit cả thư mục `Library/` (~500MB–1GB+) vào git!

---

## 2. Cấu hình .gitignore

### Nội dung .gitignore chuẩn cho Unity

Tạo file `.gitignore` ở thư mục gốc của project với nội dung sau:

```gitignore
# ═══════════════════════════════════════════
# Unity — thư mục tự sinh lại, KHÔNG commit
# ═══════════════════════════════════════════
[Ll]ibrary/
[Tt]emp/
[Oo]bj/
[Bb]uild/
[Bb]uilds/
[Ll]ogs/
[Uu]ser[Ss]ettings/
MemoryCaptures/
Recordings/

# ═══════════════════════════════════════════
# IDE — Visual Studio / Rider tự sinh
# ═══════════════════════════════════════════
ExportedObj/
.consulo/
*.csproj
*.unityproj
*.sln
*.suo
*.tmp
*.user
*.userprefs
*.pidb
*.booproj
*.svd
*.pdb
*.mdb
*.opendb
*.VC.db
.idea/
.vs/

# ═══════════════════════════════════════════
# Build output
# ═══════════════════════════════════════════
*.apk
*.aab
*.unitypackage
*.app
*.exe
*.x86
*.x86_64
*.bc

# ═══════════════════════════════════════════
# OS
# ═══════════════════════════════════════════
.DS_Store
Thumbs.db
Desktop.ini
```

### Vì sao phải loại Library/?

| Thư mục | Lý do loại |
|---------|-----------|
| `Library/` | Cache Unity build (tự sinh lại). Có thể 500MB–2GB |
| `Temp/` | File tạm, xóa khi đóng Unity |
| `Logs/` | Log build, không cần thiết |
| `UserSettings/` | Cài đặt cá nhân của từng máy |
| `*.csproj / *.sln` | IDE tự sinh lại khi mở project |

---

## 3. Git LFS cho file lớn

Git LFS (Large File Storage) dùng để lưu trữ file nhị phân lớn như texture, audio, model 3D.

### Cài đặt Git LFS

```bash
# Cài Git LFS (một lần duy nhất)
git lfs install
```

### Cấu hình .gitattributes cho Unity

Tạo file `.gitattributes` ở thư mục gốc:

```gitattributes
# Unity binary files — dùng LFS
*.png filter=lfs diff=lfs merge=lfs -text
*.jpg filter=lfs diff=lfs merge=lfs -text
*.jpeg filter=lfs diff=lfs merge=lfs -text
*.gif filter=lfs diff=lfs merge=lfs -text
*.psd filter=lfs diff=lfs merge=lfs -text
*.tga filter=lfs diff=lfs merge=lfs -text
*.tiff filter=lfs diff=lfs merge=lfs -text
*.hdr filter=lfs diff=lfs merge=lfs -text
*.exr filter=lfs diff=lfs merge=lfs -text

*.mp3 filter=lfs diff=lfs merge=lfs -text
*.wav filter=lfs diff=lfs merge=lfs -text
*.ogg filter=lfs diff=lfs merge=lfs -text

*.fbx filter=lfs diff=lfs merge=lfs -text
*.obj filter=lfs diff=lfs merge=lfs -text
*.blend filter=lfs diff=lfs merge=lfs -text
*.ma filter=lfs diff=lfs merge=lfs -text
*.mb filter=lfs diff=lfs merge=lfs -text

*.mp4 filter=lfs diff=lfs merge=lfs -text
*.mov filter=lfs diff=lfs merge=lfs -text

*.unitypackage filter=lfs diff=lfs merge=lfs -text
*.asset filter=lfs diff=lfs merge=lfs -text

# Text files — luôn dùng LF line endings
*.cs text eol=lf
*.shader text eol=lf
*.cginc text eol=lf
*.hlsl text eol=lf
*.json text eol=lf
*.xml text eol=lf
*.yaml text eol=lf merge=unityyamlmerge
*.unity text eol=lf merge=unityyamlmerge
*.prefab text eol=lf merge=unityyamlmerge
*.asset text eol=lf merge=unityyamlmerge
```

---

## 4. Workflow hàng ngày

### Trước khi bắt đầu làm việc

```bash
# Luôn pull code mới nhất về trước khi làm
git pull origin main
```

### Khi hoàn thành một tính năng

```bash
# 1. Xem file nào đã thay đổi
git status

# 2. Xem chi tiết thay đổi
git diff

# 3. Stage file muốn commit
git add Assets/Scripts/CardManager.cs
git add Assets/Prefabs/Card.prefab

# Hoặc add tất cả file
git add .

# 4. Commit với message mô tả rõ ràng
git commit -m "feat: add card flip animation"

# 5. Push lên GitHub
git push
```

### Quy tắc đặt commit message

```
<type>: <mô tả ngắn gọn>

type có thể là:
  feat     — Thêm tính năng mới
  fix      — Sửa bug
  chore    — Dọn dẹp, cấu hình, không ảnh hưởng gameplay
  docs     — Cập nhật tài liệu
  refactor — Cải tổ code
  style    — Format code, không đổi logic
  test     — Thêm/sửa test
```

**Ví dụ commit message tốt:**
```
feat: add card shuffling system
fix: resolve null reference in CardDealer on scene load
chore: update .gitignore to exclude UserSettings
refactor: extract CardAnimation into separate component
```

---

## 5. Xử lý lỗi phổ biến

---

### ❌ Lỗi: `non-fast-forward` — bị từ chối push

```
! [rejected] main -> main (non-fast-forward)
error: failed to push some refs
hint: Updates were rejected because the tip of your current branch is behind
```

**Nguyên nhân:** Remote có commit mà local chưa có.

**Cách xử lý:**

```bash
# Cách 1 — Pull về rồi push (khuyến nghị khi làm nhóm)
git pull --rebase origin main
git push

# Cách 2 — Force push (chỉ dùng khi làm một mình, chấp nhận xóa history remote)
git push origin main --force-with-lease
```

> ⚠️ **KHÔNG** dùng `--force` khi làm nhóm — sẽ xóa commit của người khác!

---

### ❌ Lỗi: Push bị treo / mất rất nhiều thời gian

**Nguyên nhân:** Repository quá lớn do đã commit `Library/` hoặc file nhị phân lớn.

**Chẩn đoán:**
```bash
git count-objects -vH
# Nếu size-pack > 100MB → có vấn đề
```

**Cách xử lý — Xóa thư mục khỏi tracking:**
```bash
# Untrack Library và các thư mục Unity generated
git rm -r --cached Library/ Logs/ UserSettings/ Temp/

# Commit thay đổi
git commit -m "chore: remove Unity generated folders from git tracking"

# Push (có thể vẫn chậm lần này vì history cũ)
git push
```

**Cách xử lý triệt để — Tạo lại lịch sử sạch (orphan branch):**
```bash
# Tạo branch mới không có history
git checkout --orphan clean-main

# Add tất cả file (đã có .gitignore nên sẽ bỏ qua Library/)
git add -A
git commit -m "Initial clean commit"

# Push đè lên remote
git push origin clean-main:main --force

# Đồng bộ local main
git checkout main
git reset --hard clean-main
git branch -D clean-main
```

---

### ❌ Lỗi: Merge conflict trên file `.unity` / `.prefab`

**Nguyên nhân:** Unity dùng định dạng YAML cho scene và prefab — khó merge tự động.

**Cách xử lý:**

```bash
# Cách 1 — Giữ version của mình
git checkout --ours Assets/Scenes/GameScene.unity
git add Assets/Scenes/GameScene.unity

# Cách 2 — Lấy version remote
git checkout --theirs Assets/Scenes/GameScene.unity
git add Assets/Scenes/GameScene.unity

# Hoàn tất merge
git commit
```

**Cách phòng tránh:** Mỗi người chịu trách nhiệm các scene/prefab khác nhau, tránh edit cùng file.

---

### ❌ Lỗi: Commit nhầm file lớn / nhạy cảm

```bash
# Xóa file khỏi commit cuối (chưa push)
git reset HEAD~1
git rm --cached duong-dan/file-nham.asset
git add .
git commit -m "fix: remove large file accidentally committed"

# Nếu đã push — cần rewrite history (nguy hiểm, cẩn thận)
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch duong-dan/file-nham.asset" \
  --prune-empty --tag-name-filter cat -- --all
git push origin main --force
```

---

### ❌ Lỗi: `Authentication failed`

```
remote: Support for password authentication was removed
fatal: Authentication failed for 'https://github.com/...'
```

**Cách xử lý — Dùng Personal Access Token (PAT):**

1. Vào GitHub → **Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic)**
2. **Generate new token** → chọn scope `repo`
3. Copy token
4. Chạy lệnh sau và nhập token thay mật khẩu:

```bash
git remote set-url origin https://<username>:<TOKEN>@github.com/<username>/<repo>.git
```

Hoặc dùng Git Credential Manager (Windows):
```bash
git config --global credential.helper manager
```

---

## 6. Làm việc nhóm

### Thiết lập branch strategy

```
main        ← Branch chính, luôn ổn định
develop     ← Branch phát triển chính
feature/*   ← Tính năng mới (VD: feature/card-animation)
fix/*       ← Sửa bug (VD: fix/dealer-null-ref)
```

### Workflow với branch

```bash
# Tạo branch mới cho tính năng
git checkout -b feature/card-animation

# Làm việc, commit như bình thường
git add .
git commit -m "feat: add card flip animation"

# Push branch lên GitHub
git push origin feature/card-animation

# Tạo Pull Request trên GitHub để review
# Sau khi được approve → merge vào develop/main
```

### Pull Request checklist

- [ ] Code chạy không có lỗi trong Unity
- [ ] Scene và Prefab không có conflict
- [ ] Không commit `Library/`, `Logs/`, `UserSettings/`
- [ ] Commit message rõ ràng, đúng format
- [ ] Đã test tính năng trước khi tạo PR

---

## 7. Khôi phục dự án

### Xem lịch sử commit

```bash
# Xem log ngắn gọn
git log --oneline -20

# Xem log với graph (nhánh)
git log --oneline --graph --all -20

# Xem thay đổi trong một commit cụ thể
git show <commit-hash>
```

### Quay về commit cũ

```bash
# Xem file ở commit cũ (không thay đổi working directory)
git show <commit-hash>:Assets/Scripts/CardManager.cs

# Khôi phục một file cụ thể về commit cũ
git checkout <commit-hash> -- Assets/Scripts/CardManager.cs

# Quay toàn bộ project về commit cũ (nguy hiểm — mất work hiện tại)
git reset --hard <commit-hash>
```

### Stash — lưu tạm thay đổi chưa commit

```bash
# Lưu tạm thay đổi
git stash

# Lấy lại thay đổi đã stash
git stash pop

# Xem danh sách stash
git stash list
```

---

## 📌 Tổng kết — Checklist thiết lập dự án Unity mới

```
□ 1. Tạo repository GitHub với .gitignore template Unity
□ 2. Clone về máy
□ 3. Copy project Unity vào thư mục đã clone
□ 4. Kiểm tra .gitignore đã có Library/, Temp/, Logs/
□ 5. Tạo .gitattributes cho Git LFS (nếu có file lớn)
□ 6. git lfs install (nếu dùng LFS)
□ 7. git add . && git commit -m "Initial commit"
□ 8. git push -u origin main
□ 9. Kiểm tra GitHub xem file đã lên chưa
□ 10. Đảm bảo KHÔNG có thư mục Library/ trên GitHub
```

---

*Tài liệu được tạo ngày 16/09/2026 — Dự án CardGame*
