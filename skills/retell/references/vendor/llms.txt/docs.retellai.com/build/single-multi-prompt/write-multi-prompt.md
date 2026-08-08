> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Build a multi-prompt agent

> Build a Retell multi-prompt agent by breaking the conversation into prompted states with explicit transitions between them — like a lead qualification flow.

<Frame>
  <img height="300" src="https://mintcdn.com/retellai/M9QYKZE4hbt00HfL/images/multi.png?fit=max&auto=format&n=M9QYKZE4hbt00HfL&q=85&s=6e40e61c6d297d119c4983acc9071f9e" alt="Multi Prompt Agent" data-path="images/multi.png" />
</Frame>

<Steps>
  <Step title="Break down the conversation into steps">
    In each step, you can define a prompt.
  </Step>

  <Step title="Define state transition logic">
    Just like in the "Lead qualification" template, you can have this prompt to define when to transition to the next step like if users say "yes" to the question, transition to `your_next_state` state.

    ```
    7. Ask if user is interested in an in person tour.
     - if yes, transition to schedule_tour.
     - if no or hesitant, call function end_call to hang up politely and say will reach out if any other interesting properties pop up.
    ```
  </Step>

  <Step title="Define when to call the function">
    Just like in the "Lead qualification" template, you can have this prompt to define when to call the function like call the `your_function_name` function to book the appointment.

    ```
    3. Confirm the date, time, and timezone selected by user: "Just to confirm, you want to book the appointment at ...". Make sure this is a time from the available slots.
    4. Once confirmed, call function book_appointment to book the appointment.
    ```
  </Step>
</Steps>

### Video tutorial

<iframe width="360" height="200" src="https://www.youtube.com/embed/fowitg78XGQ?si=brUOa58Icxeelp88" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen />

See community templates in [docs](https://docs.google.com/document/d/1hx6hdTEjAR4y4xXZ7RLMH2byQNVW1ABxC8S4FwvTx_Y/edit?tab=t.0#heading=h.wf5bktkelope)
