# Push This Project to GitHub

Your code is committed locally. Follow these steps to create a new GitHub repo and push everything.

## 1. Create a new repository on GitHub

1. Go to [https://github.com/new](https://github.com/new).
2. **Repository name:** `health-analytics-nhanes-2017-2020` (or any name you prefer).
3. **Description (optional):** e.g. `Health analytics and classification using NHANES 2017–2020 data: data wrangling, health scoring, ML (Ordinal LR, Random Forest), SMOTE, visualizations.`
4. Choose **Public**.
5. **Do not** initialize with a README, .gitignore, or license (this repo already has them).
6. Click **Create repository**.

## 2. Add the remote and push

In a terminal, from this project folder run (replace `YOUR_USERNAME` with your GitHub username):

```bash
cd "c:\Users\kalra\Downloads\Health Analytics and Classification using NHANES 2017–2020 Data"

git remote add origin https://github.com/YOUR_USERNAME/health-analytics-nhanes-2017-2020.git

git branch -M main
git push -u origin main
```

If your default branch is already `master` and you want to keep it:

```bash
git remote add origin https://github.com/YOUR_USERNAME/health-analytics-nhanes-2017-2020.git
git push -u origin master
```

## 3. Set your Git identity (if you haven’t)

If you want your name and email on future commits:

```bash
git config --global user.email "your-email@example.com"
git config --global user.name "Your Name"
```

---

After pushing, your repo will contain:

- README.md (detailed project description)
- Jupyter notebook (full analysis)
- All 5 NHANES datasets (P_*.xpt)
- Both PDFs
- requirements.txt
- .gitignore
