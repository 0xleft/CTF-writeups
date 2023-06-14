# rev/Mypin

## By heith

## Description

I made a safe with a pin of only two digits. Author: Heith

Attachments:
[My-pin.jar](https://ctf.n00bzunit3d.xyz/files/7bc6dda38448c8ffd5c038f29007d26c/My-pin.jar)

## Solution

This is a java challenge! <3

Firstly we can extract the underlying .class files from the archive and then use a tool like IntelliJ to view the bytecode as if it was normal java code.
(IntelliJ uses FernFlower as the decompiler)

Decompiled:
```
- Mypin.java
- PinButton.java
- ResetButton.java
- Secret.java
```

Mypin is the entry class with psvm. It initializes two buttons one and zero. When you click on a button this gets executed:

PinButton.java
```java
public void actionPerformed(ActionEvent var1) {
    Secret.getInstance().process(this.getText().charAt(0));
    this.app.updateOutput();
}
```

Secret.java
```java
public void process(char var1) {
    if (this.cnt <= 9) {
        int var2 = this.mydata.length / 9;

        for(int var3 = 1; var3 <= var2; ++var3) {
            int var4 = 9 * var3 - this.cnt;
            int var5 = this.box[var3 - 1] + this.mydata[var4];
            var5 += var1 - 48;
            this.mydata[var4] = var5 % 2;
            if (var5 >= 2) {
                this.box[var3 - 1] = 1;
            } else {
                this.box[var3 - 1] = 0;
            }
        }

        ++this.cnt;
    }
}
```

## Code explanation

We can see that when a button is clicked the number of the button gets processed. The very interesting part is that the process function only cares to process the button press if the count is less then `10` meaning this is a very short pin.

We can easily brute force this.

## Exploit

```java
while (true) {
    output = Secret.getInstance().generateNumber();
    for (int i = 0; i < 9; i++) {
        System.out.println(output.charAt(i));
        Secret.getInstance().process(output.charAt(i));
        String data = Secret.instance.getData();
        System.out.println(data);
        if (Secret.getInstance().check(data)) {
            System.out.println("FOUND");
            System.out.println(data);
            System.out.println(output);
            System.exit(0);
        }
    }
    Secret.getInstance().resetInstance();
}
```

We are basically generating a random 9-digit number as a string and then pressing those buttons. Eventually we get the flag.

Thank you Heith for this amazing challenge :)!

# FLAG CAPTURED!