# First compile attempt

The built-in Tectonic compiler rejected the first v09 QA source because its temporary compilation directory could not resolve the relative `../../generated/...` paths. The source and failure diagnostics are retained in the turn log. The retry source uses absolute paths only for this standalone QA artifact; the paper sources keep repository-relative paths.
