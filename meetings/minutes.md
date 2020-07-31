
Celeritas Meeting minutes
=========================


# Fri Mar 20, 2020

Tom presented current budget situation.
FWP is in process and the current document is on Celeritas' General Slack
channel.

Seth described the two milestones for current FWP:

- VecGeom GPU verification
- GPU performance demonstration of EM physics using a gdml geometry

Seth started a Rosetta Stone for nomenclature between Nuclear and HEP physics to
avoid confusion as we move forward. File is available on GitHub.

Krzysztof suggested talking to Soon Yung about CMS MC. Stefano will work on it.

FNAL has funding for Guilherme and Soon Yung to work full time on the project.

Philippe asked about the computing environment. Seth suggested keeping it open
and use spack.

For documentation we will use GitHub, at least for now.

Slides for the CERN Geant4 R&D meeting will be shared soon.



# Fri Mar 27, 2020

Discussed extension of celeritas requirement list:
- List is ample, but it is the needed equivalent information for HEP experiments.
- Description is very general, therefore Kevin Pedro may join the meeting next
  week to help clarifying the requirements.

CERN Geant4 R&D meeting:
- Team decided to aim for Apr 14 instead of Mar 31st.
- Focus of the talk so far is to present the team, describe the scope of the
project, and show openness to engage with the community.
- Slides will be sent around for appraisal.


# Fri Apr 3, 2020

Kevin Pedro and Sunanda Banerjee were present. Takeaways of the discussion:

- Current main bottleneck on CMS is disk I/O.
- Habilty to produce full track information is necessary for full adoption in
the long run.
- Keeping hits of sensitive volumes is enough for many cases.
- SIM+DIGI can be done using in-memory information.

**Meeting time changed to every Wednesday, 2 pm, Eastern Time.**
- Kevin and Sunanda are more than welcome to join regularly, but may be present
once every 2 weeks.


# Wed Apr 8, 2020

- Discussion about Geant4 gdml input, and possible routes to parse the data,
since VecGeom does not handles the materials and specified energy cuts.
- CERN Geant4 R&D meeting:
  - 2 talks will be presented:
    - Monte Carlo neutron transport in the ECP Coupled Monte Carlo Neutronics
    and Fluid Flow Simulation of Small Modular Reactor (ExaSMR) project
    - Celeritas: An accelerated charged-particle detector simulation application
    for LHC experiments
  - Slides for the first talk are already on Slack (# general) for appraisal.
  - Slides for the second talk will be available soon.


# Wed Apr 15, 2020

- Guilherme is trying to compile `vecGeom-test` and is running into an error
where headers are searched as `"root/TRootHeader.h"` instead of simply
`TRootHeader`.
- Seth is having a null pointer issue when using the CUDA NavigationPool.
- Discussion about the implications of having all the physics code connected to
the transport loop. The larger the code, the more is the usage of on device
registry, which decreases occupancy.
- Recap on Tom's presentations in the Geant4 Working Group meeting on Apr 14 for
the ones who could not make it. The highlights are:
  - Overall good reception.
  - Community is aware of the challenges and agrees that tackling only part of
  the physics is valuable.
  - Being able to simulate EM showers in the calorimeter is a good milestone.


# Wed Apr 22, 2020

- Guilherme was able to help Seth fix the null pointer issue from last week.
Details on Slack (#vecgeom).
- Seth is not sure how to use `vgdml::Frontend::Load(gdml_filename)`. P. Canal
will look into that.
- Infrastructure build for the transport loop will start soon.


# Wed Apr 29, 2020

- Seth briefly mentioned about his meeting with Amanda, Stefano, and Tom this
morning, which addressed the first steps of the project. Issues were opened in
<https://github.com/celeritas-project/celeritas>.
- Seth asked about the geantino (a non-interacting particle used to
test the transporting mechanism), and if setting all materials to vacuum should
produce the same results (a similar approach is used in Shift).
  - Guilherme: Yes.
- Discussion regarding continuous energy processes in the case of large steps,
which would cause a considerable discrepancy with the correct cross-section
value at the end of the step.
  - Philippe: There is nothing in the code to prevent that to happen, although
  it is believed that this is not an issue in any currently used model.
- Seth asked if particles have different weights in Geant4.
  - Not by default. Biasing exists, but weighting is set to 1 for every particle
  as a standard. CMS has implemented biasing, but this question will need to be
  followed up by CMS people – which were not present in the meeting.


# Wed May 6, 2020
**Meeting adjourned.**


# Wed May 13, 2020

- FWP was signed by Nichols and Marcel. DOE status is TBD. Tom will look into it.  
- Celeritas Startup branch now has initial infrastructure, compiling with CUDA
and Google test (albeit with no tests yet).  
- Seth, Amanda, and Stefano should start working on infrastructure tasks on
Thursday.
- Philippe: What is the plan to converge?
  - A clearer big picture still needs to be defined. For example, Seth prefers
  to use Thrust, which is not used in GeantX.


# Wed May 20, 2020
**Meeting adjourned.**


# Fri May 29, 2020

- Soon briefly updated the team on the HSF simulation workgroup meeting. More at
<https://indico.cern.ch/event/921244/>.
- Amanda worked on the random number generator and will push the update.
- Stefano is implementing a test physics process and will also push the update
soon. A header with constants and units was also created. The team prefers to
add new information as needed, instead of just include CLHEP data.
- Seth will merge celeritas startup branch into master.
- Long discussion regarding the system of units to be used by the Celeritas
internal code. There is not a fully agreed path forward, but an internally
consistent system of units (CGS), with input/output conversion from/to different
applications is preferred by Seth and Tom. Team will revisit the topic as coding
evolves.
- A Slack Poll will be sent to choose a new Celeritas meeting date. The team
also may start a weekly coding meeting on top of the existing Celeritas meeting.
The coding one will probably gather less people and will focus on coding and
technical discussions.


# Wed Jun 3, 2020

- Seth suggests new meeting schedules. Coding meetings will happen on Mondays
and Thursdays at 1 pm (CT) / 2 pm (ET), with biweekly Celeritas meetings taking
place every other Friday at 1 pm (CT) / 2 pm (ET) for more general discussions.


# Fri Jun 12, 2020

- Discussion focused on physics tables produced by Geant4. Several processes
use tables calculated at the beginning of the simulation instead of performing
on the fly calculation at each simulation step.
- Tables are created every time Geant4 is run and can be dumped to disk as ascii
files. This can be done by using the following options in the Geant4-Sandbox:

```
/run/particle/storePhysicsTable PATH_TO_TARGET_STORE_DIR
/run/particle/setStoredInAscii 1
/run/particle/applyCuts true
/run/particle/dumpCutValues all
```

- Tables are calculated based on the loaded physics list and materials from the
gdml file.
- Celeritas will use such tables, which will be pre-generated by Geant4.
- Track and geometry management needs to get started. Seth will start a new
coding meeting with Guilherme on Tuesdays at 2 pm (ET).


# Fri Jul 17, 2020

- Seth inquired about members' availability:
  - Stefano: Full time.
  - Guilherme: 60%-80%.
  - Krzysztof: Monitoring/consulting.
  - Philippe: 50% on Geant related projects (including Celeritas).
  - Sun Yung: Currently at 40%, but will ramp up to 80%.
  - Paul: Monitoring until next FY.
  - Amanda: 90% to full time.
  - Vincent: To be determined.

- Tom's update on funding:
  - Fermilab has received funding for Guilherme and Sun.
  - ORNL still unclear. We should have an update by the next meeting.

- Seth's update on the current status:
  - Data cpu/gpu infrastructure has been set up.
  - Random number generator is being developed.
  - Nomenclature is still being discussed, but good progress has been made.
  - Base code for dealing with geometry, material information, and physics
  processes is converging.

- Paul: How the memory management will be done for secondaries?
  - Seth: current idea is a stack allocator. Code pre-computes the amount of
  secondaries needed to be allocated for every primary and requests allocation
  from the stack. If there is enough memory, it moves forward, otherwise the
  stack allocator returns a null pointer and the physics process exits. In that
  case, the remaining particles will be moved to a queue so they can be
  transported by the next kernel launch.


# Fri Jul 31, 2020

- Tom's funding update:
  - Expected to receive funding for the next FY before September.

- Project's update:
  - Added new code design to Celeritas wiki, everyone is encouraged to provide
  feedback.
  - Seth wrote a new test harness for the KNInteractor.
  - Guilherme is working on vecgeom's magfield propagator.
  - Stefano is working on code to export Geant4 particle and physics table data
  and import it into Celeritas.
  - Amanda is working on random number generators.

- Upcoming HEP update in September. We plan to present a few micro examples on
how the current code is performing.
- Soon suggests a step-by-step installation process sketched up.
