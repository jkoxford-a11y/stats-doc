# R Setup Guide

This guide will get you set up with R for Statistics 224.

## 🌟 Recommended: Posit Cloud

**Best for:** Everyone, especially beginners

### Why Posit Cloud?
✓ No installation needed  
✓ Works on any computer (even Chromebooks)  
✓ Consistent environment for everyone  
✓ Access from anywhere  
✓ Free tier available  

### Setup Steps

1. **Go to:** [posit.cloud](https://posit.cloud/)
2. **Sign up** with your school email
3. **Create a new project**
4. **You're ready!**

!!! success "That's it!"
    Posit Cloud comes with R and RStudio pre-installed. No configuration needed.

---

## 💻 Alternative: Install Locally

**Best for:** Advanced users who want offline access

### Step 1: Install R

=== "Windows"
    1. Go to [CRAN](https://cran.r-project.org/)
    2. Click "Download R for Windows"
    3. Click "base"
    4. Download and run the installer
    5. Accept all defaults

=== "Mac"
    1. Go to [CRAN](https://cran.r-project.org/)
    2. Click "Download R for macOS"
    3. Download the `.pkg` file
    4. Open and install
    5. Accept all defaults

=== "Linux"
    ```bash
    # Ubuntu/Debian
    sudo apt update
    sudo apt install r-base
    
    # Fedora
    sudo dnf install R
    ```

### Step 2: Install RStudio

1. Go to [posit.co/download/rstudio-desktop](https://posit.co/download/rstudio-desktop/)
2. Download RStudio Desktop (Free)
3. Install with default settings
4. Launch RStudio

---

## 📦 Essential Packages

Install these packages in R:

```r
# Copy and paste this into R console
install.packages(c(
  "tidyverse",    # Data manipulation
  "effsize",      # Effect sizes
  "effectsize",   # More effect sizes
  "car",          # ANOVA tools
  "lsr",          # Additional stats
  "psych"         # Descriptive stats
))
```

!!! tip "Run Once"
    You only need to install packages once. After that, load them with `library()`.

---

## 🧪 Test Your Setup

### Quick Test

Run this code to verify everything works:

```r
# Create some data
x <- c(1, 2, 3, 4, 5)
y <- c(2, 4, 6, 8, 10)

# Simple plot
plot(x, y, main = "Test Plot", col = "blue", pch = 19)

# Simple statistics
mean(x)
sd(y)
cor(x, y)

# If this runs without errors, you're good! ✓
```

**Expected output:**
- A plot with blue dots
- Numbers printed: 3, 2.236..., 1

---

## 📂 Organizing Your Work

### Create Project Structure

```
Stats224/
├── data/           # Your datasets
├── scripts/        # R code files
├── output/         # Plots and results
└── notes/          # Your notes
```

### Best Practices

!!! tip "R Tips"
    - **Use R Scripts** (.R files) not console
    - **Comment your code** with `#`
    - **Save frequently**
    - **Use meaningful variable names**
    - **One analysis per script**

---

## 🔧 RStudio Interface

### Main Panels

```
┌─────────────────────┬─────────────────────┐
│ Source (Scripts)    │ Environment/History │
│                     │                     │
│ Your code here      │ Your data objects   │
│                     │                     │
├─────────────────────┼─────────────────────┤
│ Console             │ Files/Plots/Help    │
│                     │                     │
│ Code runs here      │ Output appears here │
│                     │                     │
└─────────────────────┴─────────────────────┘
```

### Keyboard Shortcuts

| Action | Windows/Linux | Mac |
|--------|---------------|-----|
| Run line | Ctrl+Enter | Cmd+Enter |
| New script | Ctrl+Shift+N | Cmd+Shift+N |
| Save | Ctrl+S | Cmd+S |
| Comment/Uncomment | Ctrl+Shift+C | Cmd+Shift+C |

---

## 📥 Loading Data

### From CSV File

```r
# If file is in your working directory:
data <- read.csv("mydata.csv")

# Check it loaded correctly:
head(data)      # First 6 rows
str(data)       # Structure
summary(data)   # Descriptive stats
```

### Example Datasets

R comes with built-in datasets for practice:

```r
# Load built-in dataset
data(mtcars)

# Explore it
?mtcars         # Help file
head(mtcars)    # Preview
names(mtcars)   # Variable names
```

---

## 🆘 Common Issues

### "Package not found"
**Solution:** Install it first:
```r
install.packages("package_name")
```

### "Object not found"
**Solution:** Check spelling, make sure you created it first

### "Cannot open file"
**Solution:** Check file path, use `getwd()` to see working directory

### Console shows `+` instead of `>`
**Solution:** You have incomplete code. Press `Esc` to cancel.

---

## 💡 Learning Resources

### Within R
```r
?function_name   # Help for a function
??search_term    # Search all help files
help.start()     # Open help browser
```

### External Resources
- [R for Data Science](https://r4ds.had.co.nz/) (free online book)
- [RStudio Cheatsheets](https://posit.co/resources/cheatsheets/)
- Our [Interactive Modules](../modules/overview.md)

---

## ✅ Setup Checklist

```
[ ] R installed (or Posit Cloud account created)
[ ] RStudio running
[ ] Essential packages installed
[ ] Test code runs successfully
[ ] Project folder created
[ ] Ready to start analyzing!
```

---

**Next:** [Getting Started Guide](getting-started.md) | [Decision Tree](../which-test/decision-tree.md)
