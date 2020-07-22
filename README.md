**1. Clone wallet sources**

```
git clone https://github.com/quanizzgfx/crazycoinwallet.git
```

**2. Modify `CryptoNoteWallet.cmake`**
 
```
set(CN_PROJECT_NAME "crazycoin")
set(CN_CURRENCY_DISPLAY_NAME "CrazyCoin")
set(CN_CURRENCY_TICKER "CZCO")
```

**3. Set symbolic link to coin sources at the same level as `src`. For example:**

```
ln -s ../crazycoin cryptonote
```

Alternative way is to create git submodule:

```
git submodule add https://github.com/quanizzgfx/crazycoin.git cryptonote
```

Replace URL with git remote repository of your coin.

**4. Build**

```
mkdir build && cd build && cmake .. && make
```
