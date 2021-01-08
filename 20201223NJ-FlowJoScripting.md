# 20201223NJ-FlowJoScripting

Much of this documentation is from https://docs.flowjo.com/flowjo/advanced-features/script-editor/

## General

```{JavaScript}
var samples = workspace.samples;
var exp1Group = workspace.groups['Exp. 1'];
exp1Group.samples.names;
var myGroup = workspace.groups.create("My Group");
myGroup.add(workspace.samples.slice(0, 10, 2));
var sample1 = exp1Group.samples[0];
var sampleA1 = exp1Group.samples['A1.fcs'];
```

## Help

```{JavaScript}
workspace.help;
var samples = workspace.samples;
samples.help;
var firstSample = samples[0];
firstSample.help;
math.help;
stats.help;
```

## Sample Keywords

The script editor can overwrite keywords within the context of the current workspace.

```{JavaScript}
var fil = sampleA1.keywords['$FIL'];
var withoutExt = fil.substring(0, fil.lastIndexOf);
sampleA1.keywords['Custom Keyword'] = withoutExt;
sample.keywords['PATIENT_ID'] = filename.substring(4, 8);
```

## Sample Parameters

```{JavaScript}
var sample = workspace.samples;
var fscParam = sample.parameters[1];
fscParam.name;
fscParam.help;
fscParam.stain;

workspace.groups[workspace.groups.names[3]].samples[6].parameters["Comp-BB515-A"]
```

## Traversing the Gating Tree

```{JavaScript}
var samples = workspace.samples;
var live = samples.populations("Live");
var tcells = samples.populations("Live/Singlets/Lymphocytes/T Cells");
var tcells = samples.populations("**/Lymphocytes/T Cells");
var tcellsSubgates = tcells.populations("*");
var sample1 = workspace.samples[0];
var tcells = sample1.populations("**/Lymphocytes/T Cells");
var exp1Samples = workspace.groups['Exp. 1'].samples;
var exp1Lymphs = exp1Samples.populations("**/Lymphocytes");

workspace.groups[workspace.groups.names[3]].samples["MDSCs and Neutrophils_Live dead FMO.fcs"].parameters.withStain("Live_dead");
CD11b_stian = workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("CD11B").name;
```

## Creating Gates

```JavaScript
var rectangle_gate = population.gating.rectangle(“Tethered”, “PE-A”, “FITC-A”);
rectangle_gate.minX = mean_median_PE * 1.5;
rectangle_gate.maxX = percentile_80_PE + 100;
rectangle_gate.minY = control_FITC_neg_thresh;
rectangle_gate.maxY = median_PECy55 * 2;

workspace.groups[workspace.groups.names[3]].samples[0].gating.range("test","")
workspace.groups[workspace.groups.names[3]].samples[6].parameters["Comp-BB515-A"];
workspace.groups[workspace.groups.names[3]].samples["MDSCs and Neutrophils_Live dead FMO.fcs"].children[0].stat(stats.percentile("Comp-BUV496-A",90));
```

## Functions

```{JavaScript}
f = function (pop) { return pop.counts > 1000; }
workspace.samples.filter(f);

f = function (pop) { return pop.name == "APCs_ABX-2.fcs"; }; workspace.samples.filter(f)

f = function (pop) { return /MDSC.*FMO\.fcs/.test(pop.name); }; workspace.samples.filter(f)
```

## Stats

```{JavaScript}
stats.help;
workspace.groups[workspace.groups.names[3]].samples[6];
workspace.groups[workspace.groups.names[3]].samples[6].gating.range("test","").stats

stats.stddev("FSC-A");
workspace.groups[workspace.groups.names[3]].samples[6].children[0].stat(stats.cv("FSC-A"));
workspace.groups[workspace.groups.names[3]].samples[6].children[0].stat(stats.median("Comp-BB515-A"));
workspace.groups[workspace.groups.names[3]].samples[6].children[0].stat(stats.percentile("Comp-BB515-A",80));
```

## Test

```JavaScript
for (i = 0; i < workspace.groups[workspace.groups.names[3]].samples.length; i++){
	workspace.groups[workspace.groups.names[3]].samples[i].children[1].gating.range("Live", "Comp-BUV496-A", LD_gate_min, LD_gate_max);
};
for (i = 0; i < workspace.groups[workspace.groups.names[3]].samples.length; i++){
	workspace.groups[workspace.groups.names[3]].samples[i].populations("**/Scatter").gating.range("Live", "Comp-BUV496-A", LD_gate_min, LD_gate_max);
}

```

And then I almost figured out how to do the thing

```{JavaScript
var CD11b_stain = "Comp-" + workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("CD11B").name;
var APC_CD11b_gate_min = workspace.groups[workspace.groups.names[2]].samples["APCs_CD11B FMO.fcs"].populations("**")[3].stat(stats.percentile(CD11b_stain,95));

var CD45_stain = "Comp-" + workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("CD45").name;
var APC_CD45_gate_min = workspace.groups[workspace.groups.names[2]].samples["APCs_CD45 FMO.fcs"].populations("**")[3].stat(stats.percentile(CD45_stain,95));

var CD80_stain = "Comp-" + workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("CD80").name;
var APC_CD80_gate_min = workspace.groups[workspace.groups.names[2]].samples["APCs_CD80 FMO.fcs"].populations("**")[3].stat(stats.percentile(CD80_stain,95));

var CD86_stain = "Comp-" + workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("CD86").name;
var APC_CD86_gate_min = workspace.groups[workspace.groups.names[2]].samples["APCs_CD86 FMO.fcs"].populations("**")[3].stat(stats.percentile(CD86_stain,95));

var CD206_stain = "Comp-" + workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("CD206").name;
var APC_CD206_gate_min = workspace.groups[workspace.groups.names[2]].samples["APCs_CD206 FMO.fcs"].populations("**")[3].stat(stats.percentile(CD206_stain,95));

var CD209_stain = "Comp-" + workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("CD209").name;
var APC_CD209_gate_min = workspace.groups[workspace.groups.names[2]].samples["APCs_CD209 FMO.fcs"].populations("**")[3].stat(stats.percentile(CD209_stain,95));

var CD11c_stain = "Comp-" + workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("CD11C").name;
APC_CD11c_gate_min = workspace.groups[workspace.groups.names[2]].samples["APCs_Cd11C FMO.fcs"].populations("**")[3].stat(stats.percentile(CD11c_stain,95));

var LD_stain = "Comp-" + workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("Live_dead").name;
var APC_LD_gate_max = workspace.groups[workspace.groups.names[3]].samples["APCs_Live dead FMO.fcs"].populations("**")[0].stat(stats.percentile("Comp-BUV496-A",90));
var APC_LD_gate_min = 0;


var APC_panel_group = workspace.groups[workspace.groups.names[2]].samples;
for (var i = 0; i < APC_panel_group.length; i++) {
  APC_panel_group[i].populations("**")[0].gating.rectangle("Scatter", "FSC-A", "SSC-A", 15000, 100000, 1000, 50000);
  APC_panel_group[i].populations("**")[1].gating.rectangle("FSC Singlet","FSC-W","FSC-H",50000,120000,8000,100000);
  APC_panel_group[i].populations("**")[2].gating.rectangle("SSC Singlet","SSC-W","SSC-H",20000,80000,1000,100000);
  APC_panel_group[i].populations("**")[3].gating.range("Live",LD_stain,0,APC_LD_gate_max);
  APC_panel_group[i].populations("**")[4].gating.range("CD11b+",CD11b_stain,APC_CD11b_gate_min,500000);
  APC_panel_group[i].populations("**")[4].gating.range("CD45+",CD45_stain,APC_CD45_gate_min,500000);
  APC_panel_group[i].populations("**")[4].gating.range("CD80+",CD80_stain,APC_CD80_gate_min,500000);
  APC_panel_group[i].populations("**")[4].gating.range("CD86+",CD86_stain,APC_CD86_gate_min,500000);
  APC_panel_group[i].populations("**")[4].gating.range("CD45+",CD45_stain,APC_CD45_gate_min,500000);
  APC_panel_group[i].populations("**")[4].gating.range("CD206+",CD206_stain,APC_CD206_gate_min,500000);
  APC_panel_group[i].populations("**")[4].gating.range("CD209+",CD209_stain,APC_CD209_gate_min,500000);
  APC_panel_group[i].populations("**")[4].gating.range("CD11c+",CD11c_stain,APC_CD11c_gate_min,500000);
}

```

And then I completely figured out how to do the thing.

### APC Panel

```{JavaScript}
var APC_panel_group = workspace.groups[workspace.groups.names[2]].samples;
for (var i = 0; i < APC_panel_group.length; i++) {
  APC_panel_group[i].populations("**")[0].gating.rectangle("Scatter", "FSC-A", "SSC-A", 15000, 100000, 1000, 50000);
  APC_panel_group[i].populations("**")[1].gating.rectangle("FSC Singlet","FSC-W","FSC-H",50000,120000,8000,100000);
  APC_panel_group[i].populations("**")[2].gating.rectangle("SSC Singlet","SSC-W","SSC-H",20000,80000,1000,100000);
}

var CD11b_stain = "Comp-" + workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("CD11B").name;
var APC_CD11b_gate_min = workspace.groups[workspace.groups.names[2]].samples["APCs_CD11B FMO.fcs"].populations("**")[3].stat(stats.percentile(CD11b_stain,95));

var CD45_stain = "Comp-" + workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("CD45").name;
var APC_CD45_gate_min = workspace.groups[workspace.groups.names[2]].samples["APCs_Live dead FMO.fcs"].populations("**")[3].stat(stats.percentile(CD45_stain,95));

var CD80_stain = "Comp-APC-A"
var APC_CD80_gate_min = workspace.groups[workspace.groups.names[2]].samples["APCs_CD80 FMO.fcs"].populations("**")[3].stat(stats.percentile(CD80_stain,95));

var CD86_stain = "Comp-" + workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("CD86").name;
var APC_CD86_gate_min = workspace.groups[workspace.groups.names[2]].samples["APCs_CD86 FMO.fcs"].populations("**")[3].stat(stats.percentile(CD86_stain,95));

var CD206_stain = "Comp-" + workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("CD206").name;
var APC_CD206_gate_min = workspace.groups[workspace.groups.names[2]].samples["APCs_CD206 FMO.fcs"].populations("**")[3].stat(stats.percentile(CD206_stain,95));

var CD209_stain = "Comp-" + workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("CD209").name;
var APC_CD209_gate_min = workspace.groups[workspace.groups.names[2]].samples["APCs_CD209 FMO.fcs"].populations("**")[3].stat(stats.percentile(CD209_stain,95));

var CD11c_stain = "Comp-" + workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("CD11C").name;
APC_CD11c_gate_min = workspace.groups[workspace.groups.names[2]].samples["APCs_Cd11C FMO.fcs"].populations("**")[3].stat(stats.percentile(CD11c_stain,95));

var LD_stain = "Comp-" + workspace.groups[workspace.groups.names[2]].samples[0].parameters.withStain("Live_dead").name;
var APC_LD_gate_max = workspace.groups[workspace.groups.names[2]].samples["APCs_Live dead FMO.fcs"].populations("**")[0].stat(stats.percentile("Comp-BUV496-A",90));
var APC_LD_gate_min = 0;


var APC_panel_group = workspace.groups[workspace.groups.names[2]].samples;
for (var i = 0; i < APC_panel_group.length; i++) {
  APC_panel_group[i].populations("**")[3].gating.range("Live",LD_stain,0,APC_LD_gate_max);
  APC_panel_group[i].populations("**")[4].gating.range("CD11b+",CD11b_stain,APC_CD11b_gate_min,500000);
  APC_panel_group[i].populations("**")[4].gating.range("CD45+",CD45_stain,APC_CD45_gate_min,500000);
  APC_panel_group[i].populations("**")[4].gating.range("CD80+",CD80_stain,APC_CD80_gate_min,500000);
  APC_panel_group[i].populations("**")[4].gating.range("CD86+",CD86_stain,APC_CD86_gate_min,500000);
  APC_panel_group[i].populations("**")[4].gating.range("CD45+",CD45_stain,APC_CD45_gate_min,500000);
  APC_panel_group[i].populations("**")[4].gating.range("CD206+",CD206_stain,APC_CD206_gate_min,500000);
  APC_panel_group[i].populations("**")[4].gating.range("CD209+",CD209_stain,APC_CD209_gate_min,500000);
  APC_panel_group[i].populations("**")[4].gating.range("CD11c+",CD11c_stain,APC_CD11c_gate_min,500000);
}
```

### MDSC Panel

```{JavaScript}
/* MDSC Panel */

var panel_group = workspace.groups[workspace.groups.names[3]].samples;
for (var i = 0; i < panel_group.length; i++) {
  panel_group[i].populations("**")[0].gating.rectangle("Scatter", "FSC-A", "SSC-A", 15000, 100000, 1000, 50000);
  panel_group[i].populations("**")[1].gating.rectangle("FSC Singlet","FSC-W","FSC-H",50000,120000,8000,100000);
  panel_group[i].populations("**")[2].gating.rectangle("SSC Singlet","SSC-W","SSC-H",20000,80000,1000,100000);
}

f = function (pop) { return /MDSC.*FMO\.fcs/.test(pop.name); }; 
var current_group = workspace.groups.names[3];
var samp = workspace.groups[current_group].samples[5];

var CD45_stain = "Comp-" + samp.parameters.withStain("CD45").name;
var CD45_gate_min = workspace.groups[current_group].samples["MDSCs and Neutrophils_Live dead FMO.fcs"].populations("**")[3].stat(stats.percentile(CD45_stain,95));

var CD11b_stain = "Comp-" + samp.parameters.withStain("CD11B").name;
var CD11b_gate_min = workspace.groups[current_group].samples["MDSCs and Neutrophils_CD11B FMO.fcs"].populations("**")[3].stat(stats.percentile(CD11b_stain,95));

var CD33_stain = "Comp-" + samp.parameters.withStain("CD33").name;
var CD33_gate_min = workspace.groups[current_group].samples["MDSCs and Neutrophils_CD33 FMO.fcs"].populations("**")[3].stat(stats.percentile(CD33_stain,95));

var CD115_stain = "Comp-" + samp.parameters.withStain("CD115").name;
var CD115_gate_min = workspace.groups[current_group].samples["MDSCs and Neutrophils_CD115 FMO.fcs"].populations("**")[3].stat(stats.percentile(CD115_stain,95));

var Ly6C_stain = "Comp-" + samp.parameters.withStain("Ly6C").name;
var Ly6C_gate_min = workspace.groups[current_group].samples["MDSCs and Neutrophils_Ly6C FMO.fcs"].populations("**")[3].stat(stats.percentile(Ly6C_stain,95));

var Ly6G_stain = "Comp-" + samp.parameters.withStain("Ly6G").name;
var Ly6G_gate_min = workspace.groups[current_group].samples["MDSCs and Neutrophils_Ly6G FMO.fcs"].populations("**")[3].stat(stats.percentile(Ly6G_stain,95));

var LD_stain = "Comp-" + samp.parameters.withStain("Live_dead").name;
var LD_gate_max = workspace.groups[current_group].samples["MDSCs and Neutrophils_Live dead FMO.fcs"].populations("**")[0].stat(stats.percentile("Comp-BUV496-A",90));
var LD_gate_min = 0;

for (var i = 5; i < panel_group.length; i++) {
  panel_group[i].populations("**")[3].gating.range("Live",LD_stain,0,LD_gate_max);
  panel_group[i].populations("**")[4].gating.range("CD45+",CD45_stain,CD45_gate_min,500000);
  panel_group[i].populations("**")[4].gating.range("CD11b+",CD11b_stain,CD11b_gate_min,500000);
  panel_group[i].populations("**")[4].gating.range("CD33+",CD33_stain,CD33_gate_min,500000);
  panel_group[i].populations("**")[4].gating.range("CD115+",CD115_stain,CD115_gate_min,500000);
  panel_group[i].populations("**")[4].gating.range("Ly6C+",Ly6C_stain,Ly6C_gate_min,500000);
  panel_group[i].populations("**")[4].gating.range("Ly6G+",Ly6G_stain,Ly6G_gate_min,500000);
}

```

### TC Panel

This time, we will sutomatically find the FMOs and gate. This is perfect as long as the stains have the same names as the FMOs. If the FMO is not named correctly, the minimum for the gate is set to 10,000 and logs an error.

```{JavaScript}
var group_num = 4;
var start_num = 8;
var current_group = workspace.groups.names[group_num];
var panel_group = workspace.groups[workspace.groups.names[group_num]].samples;
var fmo_pattern = "NKT cells_";
var samp = panel_group[start_num];
for (var j = start_num; j < panel_group.length; j++) {
  panel_group[j].populations("**")[0].gating.rectangle("Scatter", "FSC-A", "SSC-A", 15000, 100000, 1000, 50000);
  panel_group[j].populations("**")[1].gating.rectangle("FSC Singlet","FSC-W","FSC-H",50000,120000,8000,100000);
  panel_group[j].populations("**")[2].gating.rectangle("SSC Singlet","SSC-W","SSC-H",20000,80000,1000,100000);
};
for (var j = start_num; j < panel_group.length; j++) {
  var ld_stain = "Live_dead";
  var ld_param = "Comp-" + samp.parameters.withStain(ld_stain).name;
  var live_max = workspace.groups[current_group].samples[fmo_pattern+"Live,2f,dead FMO.fcs"].populations("**")[2].stat(stats.percentile(ld_param,95));
  panel_group[j].populations("**")[3].gating.range("Live",ld_param,0,live_max);
  for (var i = 0; i < panel_group[start_num].stains.length; i++) {
    var stain = panel_group[start_num].stains[i];
    var param = "Comp-" + samp.parameters.withStain(stain).name;
    try { var gate_min = workspace.groups[current_group].samples[fmo_pattern+stain+" FMO.fcs"].populations("**")[3].stat(stats.percentile(param,95));}
    catch(err){ var gate_min = 10000; console.log("failed gate: "+stain)}
    panel_group[j].populations("**")[4].gating.range(stain+"+",param,gate_min,500000);
  };
};
```

Then I need to regex for names that do not contain "Compensation Controls_".