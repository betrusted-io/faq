# Precursor FAQ

Please see our [wiki](https://github.com/betrusted-io/betrusted-wiki/wiki) for long-form documentation.

# Which Backup Key Should I Use? BBRAM vs eFuse

The backup key protects your backups by encrypting the FPGA bitstream, which in turn contains your root keys.

There are two types of keys you can use: BBRAM or eFuse. The trade-offs of which key to use are difficult and nuanced. Please read the following carefully and make your decision.

- BBRAM keys are volatile keys that naturally reset to zero.
  - A key reset causes all data on the device to be instantly unusable
  - The device can be re-used by re-flashing a factory image and a new key using a JTAG cable
  - Pros:
    - Useful if you would rather lose data than have it in the wrong hands
    - A misplaced device will eventually zeroize its keys once the battery is fully depleted
    - Key can be rotated in case of compromise
    - Instant secure erase by zero-izing the key
  - Cons:
    - Risk of unintentional data loss (power glitch or momentary battery disconnect leads to data loss)
    - You must charge the device roughly once every couple of months to retain the keys
    - Inconvenient and less secure provisioning: requires third-party hardware to burn (Raspberry Pi + Debug HAT). This briefly exposes the key to the Raspberry Pi doing the burning.

- eFuse keys are permanent, one-time programmable keys
  - Once burned, they can never be updated
  - Pros:
    - Full self-contained burning process - no external hardware required; no risk of key disclosure
    - No risk of data loss if the battery is disconnected or discharged
  - Cons:
    - Irreversible. If the key is compromised or lost, you cannot change it. You have to buy a new device.
    - If you lose the backup key and your device is bricked, the device is lost. You have to buy a new device.
    - Difficult to test, so the flow has less test coverage.
    - A lost device would retain its data even if the battery fully discharges
