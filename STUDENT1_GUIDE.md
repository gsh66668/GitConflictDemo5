# Practical 4 – Student 1 操作指南

> 角色：Student 1（feature-red-button 分支开发者）
> 项目名：GitConflictDemo-[你的组名]
> 包名：com.example.gitconflictdemo

---

## Part 1 – 创建 GitLab 项目

### 1.1 在 Android Studio 中打开项目

用 Android Studio 打开本文件夹 `GitConflictDemo`，等待 Gradle 同步完成。

### 1.2 测试初始版本

启动 Android 模拟器，运行 app。你应该看到两个默认颜色的按钮（Button One / Button Two），点击时有 Toast 提示。

### 1.3 在 GitLab 上创建空仓库

登录你的 GitLab 账号 → New project → Create blank project：
- Project name: `GitConflictDemo-[你的组名]`
- 不要勾选 "Initialize repository with a README"（保持空仓库）
- 点击 Create project

### 1.4 初始化本地 Git 并推送初始版本

打开终端（Android Studio 底部 Terminal），在项目根目录执行：

```bash
git init
git add .
git commit -m "Create initial Android application"
git branch -M main
git remote add origin https://gitlab.com/[你的用户名]/GitConflictDemo-[你的组名].git
git push -u origin main
```

---

## Part 2 – 邀请 Student 2 加入项目

在 GitLab 项目页面：
1. 左侧菜单 → Settings → Members
2. Invite a GitLab member → 输入 Student 2 的 GitLab 用户名
3. 角色选择 Developer（或 Maintainer）
4. 点击 Invite

Student 2 需要接受邀请后，在自己电脑上执行：
```bash
git clone https://gitlab.com/[你的用户名]/GitConflictDemo-[你的组名].git
```
然后用 Android Studio 打开并运行一次，确认初始版本正常。

---

## Part 3 – 创建 feature-red-button 分支（Student 1 本人操作）

### 3.1 创建并切换分支

```bash
git checkout -b feature-red-button
```

### 3.2 修改按钮颜色为红色

打开 `app/src/main/res/layout/activity_main.xml`，给两个 Button 都加上 `android:backgroundTint="#F44336"`。

最终文件内容应该是：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    android:padding="24dp">

    <Button
        android:id="@+id/buttonOne"
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:layout_marginBottom="16dp"
        android:text="Button One"
        android:textSize="18sp"
        android:backgroundTint="#F44336" />

    <Button
        android:id="@+id/buttonTwo"
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:text="Button Two"
        android:textSize="18sp"
        android:backgroundTint="#F44336" />

</LinearLayout>
```

### 3.3 测试并提交

运行 app，确认两个按钮都是红色。然后：

```bash
git add app/src/main/res/layout/activity_main.xml
git commit -m "Change button color to red"
git push -u origin feature-red-button
```

---

## Part 6 – 在 GitLab 上合并 feature-red-button → main

1. 回到 GitLab 项目页面 → Merge requests → New merge request
2. Source branch: `feature-red-button`，Target branch: `main`
3. 点击 Compare branches and continue
4. Title 填写：`Red button feature`
5. 点击 Merge

此时 main 分支已经包含红色按钮版本。

---

## Part 7–12 后续流程（与 Student 2 协作完成）

接下来 Student 2 会从 main 拉取最新代码，创建 `feature-green-button` 分支，把按钮改成绿色 `#4CAF50`，然后推送到 GitLab。

Student 2 在 GitLab 上发起 `feature-green-button → main` 的 Merge Request 时，GitLab 会检测到合并冲突。

### 解决冲突（你们两人商量后决定）

这里给出三种方案，任选一种：

**方案 A – 保留红色（Student 1 的改动）**
最终 `activity_main.xml` 中保留 `android:backgroundTint="#F44336"`。

**方案 B – 保留绿色（Student 2 的改动）**
最终 `activity_main.xml` 中改为 `android:backgroundTint="#4CAF50"`。

**方案 C – 第三种颜色（推荐，体现"有意决策"）**
比如改成蓝色 `android:backgroundTint="#2196F3"`。

### 本地解决冲突的命令（在 Student 2 的机器上执行）

```bash
git checkout feature-green-button
git pull origin main
```

此时 Git 会报冲突，打开 `activity_main.xml`，你会看到类似：

```xml
<<<<<<< HEAD
        android:backgroundTint="#F44336"
=======
        android:backgroundTint="#4CAF50"
>>>>>>> feature-green-button
```

删掉冲突标记 `<<<<<<< HEAD`、`=======`、`>>>>>>> feature-green-button`，只保留你们决定的最终颜色。

然后：

```bash
git add app/src/main/res/layout/activity_main.xml
git commit -m "Resolve button color merge conflict"
git push origin feature-green-button
```

回到 GitLab 上 Student 2 的 Merge Request，点击 Merge。

---

## Part 13 – 最终验证

两人都执行：

```bash
git checkout main
git pull origin main
```

确认：
- 项目能正常 Build
- App 能在模拟器运行
- `activity_main.xml` 中没有 `<<<<<<<` 等冲突标记
- 按钮颜色是你们最终决定的颜色
- GitLab 仓库中能看到两个分支和完整的 commit 历史

---

## 提交作业

把 GitLab 仓库 URL 复制下来，通过 Canvas 提交。
