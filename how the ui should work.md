my backend and front end are not matched 
@contextScopeItemMention @contextScopeItemMention @contextScopeItemMention @contextScopeItemMention @contextScopeItemMention @contextScopeItemMention 
they do not match
here is what the user should see
the inis state is IDLE
the user sees the cars images and the idel metric cards
then
when the car is connected, the user sees the qr code panel and the bottons
then if they start, the car first goese to the state of locked then checking then charging
each pannels have their own upper and lowwer parts
the locked and checking has the bottons with onlu stop actuvated
the charging has the metric cards and the bottons to toggle the extended and the stop botton
and when the stoped command comes, it is a FINISHED stated and the user sees the finial state and final metrics which are shown in a time locked duration
and when ever an error ocuures they see the error panel with it's coresponding message

during each pannel, the bottons are read from the gpios in the opihardware and translated to the real meaning they have in each phase
I want to be able to control the bottons names dynamically

keep the logics simple
