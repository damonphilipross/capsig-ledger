# Capital Signal ledger anchors

Capital Signal locks research calls before their outcome and publishes every
call's hash at <https://capitalsignal.io/calls.txt>. A hash on our own server
shows a call has not been edited; it does not show when the call was written.
This repository does that part.

Once a day a GitHub Action saves a copy of `calls.txt` to `snapshots/<date>.txt`
and timestamps it with [OpenTimestamps](https://opentimestamps.org), which
commits the file's hash to the Bitcoin blockchain. The proof is the `.ots` file
beside each snapshot. Nobody, including us, can produce a valid proof for a
file that did not exist at that time.

## Check a call

1. Find the call's line (`<time> <call_id> <sha256>`) in the first snapshot
   that contains it. The snapshot's date is the latest the call can have been
   written.
2. Check the snapshot's timestamp:

   ```bash
   pip install opentimestamps-client
   ots verify snapshots/2026-11-16.txt.ots
   ```

   This needs a Bitcoin node; without one, <https://opentimestamps.org> checks
   a snapshot and its `.ots` file in the browser. A proof made in the last few
   hours reads "pending" until Bitcoin confirms it.
3. Check the call's hash against its text: <https://capitalsignal.io/verify>
   gives the command.

The snapshots also carry a digest per day, so a later snapshot that changes an
earlier day's lines is visible by comparing the two files.
