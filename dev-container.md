# Linux VS Code DevContainer

Using ubuntu-20.04 image.  The steps below are mostly pulled from `.github/build.yaml`.

## Moved to Dockerfile

This is in the `.devcontainer/Dockerfile`

```bash
sudo apt update
sudo apt install -y p7zip-full python3-pip zstd cmake
pip install --upgrade conan==1.58
```

---

## First Boot Steps

These must be done after new container initialization.

### conan profile

```bash
conan profile new default --detect
conan profile show default
```

### git

```bash
git config --global filter.lfs.required false
git config --global filter.lfs.smudge "git-lfs smudge --skip %f"
git config --global filter.lfs.process "git-lfs filter-process --skip"
```

### SDK

> See `.github/build.yaml` for Windows/macOS versions

```bash
curl -O -L "https://github.com/V-Sekai/FBXSDK-Linux/archive/refs/tags/2020.2.zip"
7z x 2020.2.zip
mkdir -p sdk
mv ./FBXSDK-Linux-2020.2/sdk .
zstd -d -r --rm ./sdk || true
```

### conan install

```bash
conan install . -i build -s build_type=Release --build fmt -s compiler.libcxx=libstdc++11 --build missing
```

---

## Build step (Make changes, run this again)

```bash
conan build -bf build .
```

---

## Test

```bash
./build/FBX2glTF --help

./build/FBX2glTF -i test.fbx -o test.glb
```

---

## Cleanup, go back to [One time SDK](#One-time-SDK) afterwards

```bash
rm -rf 2020.2.zip
rm -rf FBXSDK-Linux-2020.2
rm -rf build
rm -rf sdk
```
