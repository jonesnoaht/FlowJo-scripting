# FlowJo-scripting

## Notes

Check out the 2020***.md file. It's ugly af, and I need to break it down into actual documentation, but the substance is here. The last script is the automated one.

It takes the list of stains and then searches for FCS files with that stain and then " FMO", and then gates on the 95th percentile.

I need to change it so that it can process an entire set of groups in one go, so then I have one script that is itself an entire analysis pipeline
also, I have not yet figured out how to do polygon gates, but I'm sure someone built that in and the command is just hiding.

## Summary

Take the scripts from this markdown file and put them into the sript editor in FlowJo.

In order for the script to work, the FMO tubes have to be named "\**stain* FMO.fcs". It sets an arbitrary gate for any stains that lack FMOs or the FMO is named incorrectly. In the future, I will ask it to default to the unstained.
