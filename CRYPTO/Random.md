# Random

Category: CRYPTO

Points: xxx

Solves: xxx

>I hid my password behind an impressive sorting machine. The machine is very luck based, or is it?!?!?!?

### Solution

The challenge gives us a cpp file which we can mostly treat as a black box. All we need to know is that we enter a string of length > 10 and unique characters, and the code will randomly shuffle the chars around multiple times. If at any point the string is sorted, then we get the flag:

```cpp
bool amazingcustomsortingalgorithm(string s) {
    int n = s.size();
    for (int i = 0; i < 69; i++) {
        cout << s << endl;
        bool good = true;
        for (int i = 0; i < n - 1; i++)
            good &= s[i] <= s[i + 1];
        
        if (good)
            return true;

        random_shuffle(s.begin(), s.end());

        this_thread::sleep_for(chrono::milliseconds(500));
    }

    return false;
}

int main() {
    string s;
    getline(cin, s);
    ...

    random_shuffle(s.begin(), s.end());
    
    if (amazingcustomsortingalgorithm(s)) {
        ...
        cout << flag << endl;
    }
}
```

The problem is that `random_shuffle` will always suffle the same way with a default seed. This means when we connect to the server, our input will always be shuffled the same way. We can first submit `0123456789` and see what it maps to after the shuffle, we can then reverse the shuffle and create a preshuffled input:

```bash
$ nc challs.n00bzunit3d.xyz 10400
0123456789
4378052169
0578439216
^C
$ nc challs.n00bzunit3d.xyz 10400
0123456789
4378052169
0578439216
^C
$ nc challs.n00bzunit3d.xyz 10400
4761058239
0123456789
n00bz{5up3r_dup3r_ultr4_54f3_p455w0rd_7bffc2b26246}
```

### Flag

```n00bz{5up3r_dup3r_ultr4_54f3_p455w0rd_7bffc2b26246}```