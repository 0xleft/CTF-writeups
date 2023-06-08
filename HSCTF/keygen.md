# rev/revrevrev

## By hmmm

## Description

A file: what's the key?

Attachments:
[keygen](https://hsctf-10-resources.storage.googleapis.com/uploads/d1f4fd3e4a0935e25ef630ca70bd74d54372b84b9a86d214d731f7979b2bce9a/keygen)

## Solution

As the file is binary we can immediately open it in Ghidra.

Decompiled code:

````
undefined8 main(int param_1,long param_2)

{
  size_t sVar1;
  byte *local_18;
  byte *local_10;
  
  if ((param_1 == 2) && (sVar1 = strlen(*(char **)(param_2 + 8)), sVar1 == 0x2a)) {
    puts("dfdfdf");
    local_10 = *(byte **)(param_2 + 8);
    local_18 = &DAT_00102008;
    while( true ) {
      if (*local_10 == 0) {
        puts("Correct");
        return 0;
      }
      if ((*local_10 ^ 10) != *local_18) break;
      local_10 = local_10 + 1;
      local_18 = local_18 + 1;
    }
    puts("Wrong");
    return 1;
  }
  puts("Wrong");
  return 1;
}
````

## Code explanation

The file takes two parameters `param_1` and `param_2`. It imediatly checks if the param_1 is equal to two. Param 2 is supposed to be our flag.

By decompiling we can also see that Ghidra left us some notes:

````
                             s_fkmq<8=?=>?l'==<2'<;=>'?l<i'<l<9_00102009     XREF[1]:     main:001011e7(*)  
        00102009 66 6b 6d        ds         "fkmq<8=?=>?l'==<2'<;=>'?l<i'<l<9<h9l::::w"
                 71 3c 38 
                 3d 3f 3d 

````

Where this variable `fkmq<8=?=>?l'==<2'<;=>'?l<i'<l<9<h9l::::w` is used in an xor ^ 10. I tested it with python and got the flag.

After xor:
`de string: lag{6275745f-7768-6174-5f6c-6f636b3f0000}`

We can see that f is missing, we can add it and submit it for points!

Thanks to hmmm for this amazing challenge!

# FLAG CAPTURED!