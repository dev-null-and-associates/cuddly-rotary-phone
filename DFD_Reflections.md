# DFD Reflections
## Aaron's Reflection
This was an interesting assignment. We use STRIDE at work to
assist in our threat modeling, so I have prior experience working
with it, but I hadn't heard of the Microsoft tool to automate some
of the threat detections. I ended up talking to our Software 
Security Group to see if the company used the tool or not, but 
they mentioned they haven't used it.  

It was a great concept to have a way to go from a diagram of your
data flow to a list of potential threats in an automated way. 
Unfortunately, I think it may have been a bit of a reach to try and
automate something as dynamic as DFDs. There are countless configurations
and types of systems, that having one tool to automate everything is a 
hard goal to reach. It really showed when trying to select the objects in
the design view and only having a limited number of objects to choose from -
with most of the objects being Microsoft-centric. Once the design was done,
it was interesting to see how many threats it detected. At first I thought I
might have made a mistake, but on closer look it seemed correct. It does look
like a good place to start, but I feel like it is a tool that would need further
iterations on the STRIDE model to get a more complete picture - especially with
all the non-standard objects a diagram could be built of.  

So overall, I would say it was a good place to start from, but would need to
be expanded upon to have a more complete picture of the threat landscape.

## Mason's Reflection
Based on the expected mitigations from the STRIDE-based threat analysis, there 
are some gaps between identified threats. Several classes of threats such as 
transport security, privilege boundaries, storage integrity, anti-automation, 
and behavioral control are not addressed.

I learned a lot from this assignment, particularyl in making a DFD and using the 
MTM tool to automate threat detection. However, I do think it would possibly had 
been a bit easier for our group, had our Use/Mis-use cases been more solid. We did 
take an approach based on feedback that allowed us to use those cases better. I 
personally found the MTM tool very useful as it allowed us to automate threat detection, 
which helps see threats you may not inherently think of.

## Matthew Vandergriff Reflections
I thought this was an interesting project, in that it showed how succinct a drawing can be, 
and then putting it through the threat modeling tool, the volume of threats that can be generated was startling.
Just looking at the number of threats, it really was much bigger than what I thought would come out. 
Much bigger than what I could think of, just looking at the DFD.I thought the tool worked out well, and was fairly straight forward.
Being able to run the tool through, and what threats popped out, made me look closer at the DFD, and think about what maybe needed changing
