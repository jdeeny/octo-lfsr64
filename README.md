# octo-lfsr64
64-bit [LFSR](https://en.wikipedia.org/wiki/Linear-feedback_shift_register)-based PRNG implementation in Octo

The LFSR has a period of 2^64-1, which is hopefully plenty for a chip8 game. All 8 bytes of state are used to produce a
single byte of output, which hopefully masks patterns in the LFSR's sequence.

## Usage

If you don't want to get the same numbers every time your program runs, you will need to seed the state of the PRNG/LFSR
with some random numbers:
```octo
v0 := random 0xFF
v1 := random 0xFF
v2 := random 0xFF
v3 := random 0xFF
v4 := random 0xFF
v5 := random 0xFF
v6 := random 0xFF
v7 := random 0xFF
save lfsr_state
```

You can seed the state with a constant value if you do want the same sequuence every time.

To get a sequence of pseudo-random values, you will need advance the state and collapse the output for each
byte:
```octo
lfsr_advance_state      # This must be called to go to the next number
lfsr_collapse_output    # This turns the 64-bit state into a single 8-bit number
vE := v1                # That number is returned in v1
```

The ```lfsr_collapse_output``` function simply XORs the bytes of state together.
The output could potentially be improved with something more complicated, but output quality seems OK. (Please let me know
if you think otherwise, so it can be corrected.)

## Saving State
State can be saved with the ```lfsr_save_*``` functions and loaded with the ```lfsr_load_*``` functions.
Two memory locations (```lfsr_saved_0``` and ```lfsr_saved_1```) are provided by default.
To simplify the load/save functions, one function is used for each possible storage location.
If you need more, you will have to duplicate those functions and provide another 8-byte area to save to.
