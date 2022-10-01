Contract code size exceeds 24576 bytes.

This contract may not be deployable on mainnet. Consider enabling the optimizer (with a low "runs" value!), turning off revert strings, or using libraries.

Gas optimisation.

Consider declaring custom errors and use that In the revert statements, in order to optimise the gas consumption.

Heavy use of require statements are seen through out the project. Consider changing some of them to revert, so the users will get the exact information for the unexpected situations/failure.