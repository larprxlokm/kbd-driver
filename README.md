## Kernel Keyboard Driver
A low-level PS/2 keyboard driver designed for x86-based kernels, featuring interrupt-driven input, scancode-to-ASCII translation, and buffer management.

# Overview
This driver interfaces directly with the PS/2 controller via I/O ports 0x60 and 0x64. It utilizes hardware interrupts (IRQ 1) to process keystrokes asynchronously, ensuring minimal CPU overhead and responsive input handling.

# Features
Interrupt-Driven: Non-blocking design using IRQ 1.

Callback System: Easily register custom handlers to process input events.

Modifier Support: Tracks state for Shift, Ctrl, and Alt keys.

Buffer Management: Implements a circular buffer for reliable data queuing.

## Implementation Guide
IDT Registration: Bind the keyboard_handler_main function to the appropriate vector in your Interrupt Descriptor Table.

PIC Configuration: Ensure the Programmable Interrupt Controller is configured to send EOI signals to the Master PIC upon interrupt completion.

Mapping: Define a scancode-to-ASCII translation table to handle standard keypresses and handle break codes (release events).

Integration: Use the register_keyboard_callback function to link the driver to your kernel's input subsystem.

Usage Example
C
void my_input_handler(uint8_t scancode) {
    // Process scancode here
}

// In kernel initialization
register_keyboard_callback(my_input_handler);
