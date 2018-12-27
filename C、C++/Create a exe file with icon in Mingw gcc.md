# Create a exe file with icon in Mingw gcc


1. get a icon file name “logo.ico” (we can use python PIL)
2. create an file name “logo.rc”, contain a line:
```
1 ICON "logo.ico"
```
3. run : (windres was include in Mingw)
  `windres logo.rc logo.o`
4. link the “logo.o” file together with other object files to the final exe file.
  for example, you have a main.c
```
gcc -c main.c -o myapp.o
windres logo.rc logo.o 
gcc myapp.o logo.o -o app_with_icon.exe
```
or just
```
windres logo.rc logo.o
gcc main.c logo.o -o o
```

