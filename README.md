# Midnight Commander Patch
## mc-4.8.22: No detach hard-links before saving
```
cd /usr/src/mc-4.8.22
patch -p0 < ~/no-detach-hardlinks-mcedit-save.diff
./configure --prefix=/usr/local
make -j5 # compilation time depends on arch, ~ 0.5min
# make install
```
```
--- src/editor/editcmd.c.orig
+++ src/editor/editcmd.c
@@ -229,6 +229,7 @@
             rv = edit_query_dialog3 (_("Warning"),
                                      _("File has hard-links. Detach before saving?"),
                                      _("&Yes"), _("&No"), _("&Cancel"));
+            if (rv == 0) rv = 1;
             switch (rv)
             {
             case 0:

```
## mc-4.8.22: No detach hard-links before saving (no prompt)
```
--- src/editor/editcmd.c.orig
+++ src/editor/editcmd.c
@@ -226,9 +226,12 @@
     {
         if (this_save_mode == EDIT_QUICK_SAVE && !edit->skip_detach_prompt && sb.st_nlink > 1)
         {
+#if 0
             rv = edit_query_dialog3 (_("Warning"),
                                      _("File has hard-links. Detach before saving?"),
                                      _("&Yes"), _("&No"), _("&Cancel"));
+#endif
+            if (rv == 0) rv = 1;
             switch (rv)
             {
             case 0:
```
