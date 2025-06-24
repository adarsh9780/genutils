```python
from nicegui import ui

# Chat history container
chat_column = ui.column().classes('w-full max-w-2xl px-4')

# Function to send a message
def handle_send():
    message = input_box.value.strip()
    if message:
        # Add new chat bubble inside the chat_column
        with chat_column:
            ui.label(f'You said: {message}') \
                .classes('self-start bg-gray-100 rounded-xl p-3 my-1')
        input_box.value = ''
        # Optionally: scroll to bottom

# Build the page layout
with ui.column().classes('items-center justify-center min-h-screen'):
    ui.label("Where should we begin?").classes('text-2xl text-center my-10')
    chat_column  # Insert the chat area

    with ui.row().classes('w-full max-w-2xl px-4 mb-4'):
        with ui.element('div').classes('relative w-full'):
            input_box = ui.input(placeholder='Ask anything...') \
                .props('rounded outlined') \
                .classes('w-full pr-14')

            ui.button(icon='send', on_click=handle_send) \
                .props('flat round dense') \
                .classes('absolute bottom-1 right-1 bg-blue-500 text-white')

            input_box.on('keydown.enter', lambda _: handle_send())

# Run the NiceGUI app
ui.run()
```
