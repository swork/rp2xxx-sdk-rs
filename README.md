# rp2xxx-sdk-rs
Rust bindgen interfaces to some RPi pico-sdk and pico-examples items

_Statement of intention, little of this is yet implemented_

Aiming for an easily maintained Rust integration of maintained C for this platform's oddities, particularly their unique PIO hardware: it isn't that complicated, there are plenty of pure-Rust implementations already, but it's a feature arena commanding few eyeballs in a space where the vendor is motivated to make code work, just not Rust code.

## OneWire and friends

1wire has tight timing requirements that can be hard to meet with even a fast microcontroller busy with other tasks. `embassy-rs` offers an example of reading a single DS18B20 temperature sensor that demonstrates embassy's asynchronous capabilities, but there's none of the single-bit operations required to search the bus and no clear path to polling a network of several devices whose identifiers are already known - and no discussion I found of the care needed to avoid over- or under-running the PIO FIFOs at .await points - some just a few uS apart.

Accordingly, `pio-onewire` here presents (will present) a Rust interface to `raspberrypi/pico-examples`'s pio-onewire blocking-mode implementation, in both sync and async form. The async code should probably be run under an interrupt executor, with care taken that higher-priority interrupts clear under timings discussed in the code.
