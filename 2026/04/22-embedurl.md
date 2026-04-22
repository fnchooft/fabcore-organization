# Embedurl (2026-04-22)

## pocs/otp29/README.md

# OTP29 - New features

Link: https://www.erlang.org/news/186

## Module Structure

Each `demo_*/0` function maps to one OTP 29 feature:

| Function | Feature | Source |
| :-- | :-- | :-- |
| `demo_is_integer_guard/0` | New `is_integer/3` guard BIF | RC1 |
| `demo_multi_valued_comprehensions/0` | Multi-valued list/map comprehensions (EEP-78) | RC1 |
| `demo_rand_shuffle/0` | `rand:shuffle/1` and `rand:shuffle_s/2` | RC1 |
| `demo_io_ansi/0` | `io_ansi` VT/ANSI terminal sequences | RC2 |
| `demo_compiler_warnings_guide/0` | 4 new default compiler warnings | RC1 |

SSH security defaults (daemon shell/exec/SFTP disabled, post-quantum `mlkem768x25519-sha256`) and the native records feature (EEP-79) are documented in inline comments — they're configuration/flag changes rather than API calls.

## Key caveats

- **Native records** (EEP-79) are experimental and need `-feature(native_records, enable)` at the top of the module plus the `-enable-feature` compiler flag .
- **`compr_assign`** (variable binding in comprehensions) similarly needs its own feature flag .
- `io_ansi` output looks best on terminals that support VT sequences; in a plain pipe it will print raw escape codes .

## Test module

[View source on Github](https://github.com/fnchooft/fabcore-organization/blob/main/pocs/otp29/otp29_new_features.erl).

---

## kerl installation

```bash
$ kerl build 29.0-rc3
Erlang/OTP 29.0-rc3 (29.0-rc3) has been successfully built.

$ kerl install 29.0-rc3 /home/fnchooft/erlang/29.0-rc3
kerl_deactivate

$ kerl list builds
29.0-rc1,29.0-rc1
28.4,28.4
29.0-rc3,29.0-rc3

$ kerl delete installation 29.0-rc1
Installation '29.0-rc1' has been deleted.

$ kerl delete build 29.0-rc1
Build '29.0-rc1' has been deleted.

$ kerl list builds
28.4,28.4
29.0-rc3,29.0-rc3
```

---

## Tests

### Standard compile (most features work as-is)

```bash
erlc otp29_new_features.erl
```

### With experimental features enabled

```bash
erlc -enable-feature native_records \
     -enable-feature compr_assign \
     otp29_new_features.erl
```

## Run

```bash
erl -noshell -eval "otp29_new_features:run_all()" -s init stop
```


## Links

 - https://www.erldocs.com/18.0/stdlib/rand.html
 - https://www.erlang.org/docs/19/man/rand.html
 - https://www.erlang.org/docs/22/man/rand
 - https://erlang.org/documentation/doc-17.0-rc1/lib/stdlib-8.0/doc/html/rand.html
 - https://elixirforum.com/t/random-based-on-rand-not-random/1518
 - https://hashrocket.com/blog/posts/the-adventures-of-generating-random-numbers-in-erlang-and-elixir
 - http://zxq9.com/erlang/docs/reg/18.0/lib/stdlib-2.5/doc/html/rand.html
 - https://manpages.ubuntu.com/manpages/bionic/en/man3/random.3erl.html
 - https://manpages.debian.org/unstable/erlang-manpages/rand.3erl.en.html
 - https://cloud.tencent.com/developer/section/1125433
 - https://runebook.dev/en/docs/erlang/lib/stdlib-5.0.1/doc/html/rand
 - https://blog.noredink.com/post/147949678708/functional-randomization
 - https://github.com/erlang/otp/blob/master/lib/stdlib/src/rand.erl
 - https://www.erldocs.com/r14b01extra/stdlib/random.html
 - https://www.erldocs.com/current/stdlib/rand


