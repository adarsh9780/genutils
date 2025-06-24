```python
from nicegui import ui

# Chat history container
chat_column = ui.column().classes('w-full max-w-2xl px-4')

# Function to handle sending messages
def handle_send():
    message = input_box.value.strip()
    if message:
        chat_column.add(
            ui.label(f'You said: {message}').classes('self-start bg-gray-100 rounded-xl p-3 my-1')
        )
        input_box.value = ''

# Page layout
with ui.column().classes('items-center justify-center min-h-screen'):

    ui.label("Where should we begin?").classes('text-2xl text-center my-10')

    chat_column

    with ui.row().classes('w-full max-w-2xl px-4 mb-4'):
        with ui.element('div').classes('relative w-full'):
            # Input box with padding-right to avoid overlap with the button
            input_box = ui.input(placeholder='Ask anything...') \
                .props('rounded outlined') \
                .classes('w-full pr-14')  # right padding for send button space

            # Absolute send button inside the input box
            ui.button(icon='send', on_click=handle_send) \
                .props('flat round dense') \
                .classes('absolute bottom-1 right-1 bg-blue-500 text-white')

            # Also allow Enter key to send
            input_box.on('keydown.enter', lambda _: handle_send())

# Start the app
ui.run()

```
