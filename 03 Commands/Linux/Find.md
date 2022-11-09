# owners 
-name 
-user 
-nouser
# type
-type f(FILE) / d (Directory) / l (symlink)
# size 
-size 8            # Exactly 8 512-bit blocks 
-size -128c        # Smaller than 128 bytes
-size 1440k        # Exactly 1440KiB
-size +10M         # Larger than 10MiB
-size +2G          # Larger than 2GiB
# actions
-exec rm {} \;
-print
-delete
-ls to run ls on the results


# examples
find / -newermt "DATE" ! -newermt "DATE" # files modified between 2 days
