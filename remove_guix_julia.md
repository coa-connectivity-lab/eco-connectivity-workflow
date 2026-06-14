# Remove guix influence on julia

Here’s a **clean, end-to-end workflow** to (1) remove Guix influence, (2) fix Julia’s SSL/registry setup, and (3) run your `run_omniscape.jl` successfully. ---

## 🧭 Goal

You want:

* ❌ no Guix interference
* ✅ Julia using system certificates (Ubuntu)
* ✅ working Julia package registry
* ✅ working Omniscape environment
* ▶️ successful `julia run_omniscape.jl`

---

## 1. 🔧 Remove Guix influence (temporary + permanent)

### 1A — Clean current shell (immediate fix)

Run:

```bash
unset SSL_CERT_FILE
unset JULIA_SSL_CA_ROOTS_PATH
```

Set correct Ubuntu certificates explicitly:

```bash
export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
export JULIA_SSL_CA_ROOTS_PATH=/etc/ssl/certs/ca-certificates.crt
```

#### Why this matters

Julia uses these variables for HTTPS/Git cloning.
Guix had pointed them to a broken certificate store earlier.

---

### 1B — Remove Guix from your shell permanently

Run:

```bash
cp ~/.bashrc ~/.bashrc.backup.$(date +%Y%m%d_%H%M%S)

sed -i '/guix/d' ~/.bashrc
sed -i '/SSL_CERT_FILE.*guix/d' ~/.bashrc
sed -i '/JULIA_SSL_CA_ROOTS_PATH.*guix/d' ~/.bashrc
sed -i '/guix-profile/d' ~/.bashrc
```

Then reload:

```bash
source ~/.bashrc
```

### Why this matters

Prevents Guix from coming back every new terminal session.

---

## 2. 🔍 Verify system certificates

Make sure Ubuntu certs exist:

```bash
ls -l /etc/ssl/certs/ca-certificates.crt
```

Expected:

* file exists
* readable
* ~200KB+

---

## 3. 🧪 Test Julia SSL + registry access

Run:

```bash
julia -e 'using Pkg; Pkg.Registry.status()'
```

### Expected result:

You should see:

```
Registry Status
 [23338594] General
```

If this works → your environment is fixed.

---

# 4. 📦 Fix your Julia project (important cleanup)

Go to your project:

```bash
cd ~/Documents/myData/data-management/omniscape-stack
```

Replace `setup.jl` with a clean version:

```bash
cat > setup.jl <<'EOF'
import Pkg

Pkg.activate(@__DIR__)

Pkg.instantiate()

Pkg.add([
    "ArchGDAL",
    "GDAL",
    "Omniscape"
])

Pkg.precompile()
EOF
```

### Why this matters

We removed:

```julia
Pkg.Registry.add("General")
```

because:

* it was failing due to SSL earlier
* it is unnecessary on normal Julia installs
* it can safely be skipped

---

## 5. 🚀 Install environment

Run:

```bash
julia setup.jl
```

### What should happen:

* Julia resolves dependencies
* downloads packages from GitHub
* builds GDAL bindings
* creates `Manifest.toml`
* precompiles everything

---

## 6. ▶️ Run Omniscape

Now execute:

```bash
julia run_omniscape.jl
```

Expected output:

```
✔ Omniscape environment loaded successfully
Julia version: 1.10.11
```

---

## 7. 📁 What “success” looks like

Your folder should now contain:

```text
omniscape-stack/
├── Project.toml
├── Manifest.toml   ✅ created
├── setup.jl
├── run_omniscape.jl
```

---

## 8. 🧠 What was actually broken (important insight)

You had **3 layered issues**:

### 1. Guix certificate override

* Julia tried using:

  ```
  ~/.guix-profile/etc/ssl/certs/ca-certificates.crt
  ```
* That file was broken → SSL failure

### 2. Git SSL failure

* Julia could not clone:

  ```
  https://github.com/JuliaRegistries/General.git
  ```

### 3. Unnecessary registry command

* `Pkg.Registry.add("General")` triggered the failure
* but it was not required anyway

---

## 9. 🧼 Final “clean state checklist”

Run:

```bash
env | grep SSL
```

You should ONLY see:

```
/etc/ssl/certs/ca-certificates.crt
```

No `.guix-profile` anywhere.


