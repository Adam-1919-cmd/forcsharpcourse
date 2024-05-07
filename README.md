# forcsharpcourse
This is  for a c sharp course. but if you want you can copy some of the code i guess











using System;
using System.Threading;
using System.Windows.Forms
using System.Runtime.InteropServices;
using System.ComponentModel;

namespace wompwomp
{
    public partial class Form1 : Form
    {
        [DllImport("user32.dll")]
        static extern short GetAsyncKeyState(Keys vKey);

        [DllImport("user32.dll", CharSet = CharSet.Auto)]
        public static extern void mouse_event(int dwFlags, int dx, int dy, int cButtons, int dwExtraInfo);

        private const int LEFTDOWN = 0x0002;
        private const int LEFTUP = 0x0004;
        private int intervals = 5; // Default interval for autoclicking
        private bool click = true; // Autoclicking state
        private int parsedValue;

        public Form1()
        {
            InitializeComponent();
            CheckForIllegalCrossThreadCalls = false; // Should be set with caution
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            Thread AC = new Thread(AutoClick);
            AC.Start(); // Start the autoclick thread

            // Ensure the event handler is assigned only once
            backgroundWorker1.DoWork -= BackgroundWorker1_DoWork; // Detach before attaching to prevent duplication
            backgroundWorker1.DoWork += BackgroundWorker1_DoWork;
            backgroundWorker1.RunWorkerAsync(); // Start the background worker
        }

        private void AutoClick()
        {
            while (true) // Infinite loop; ensure proper thread handling
            {
                if (click)
                {
                    // Simulate mouse click
                    mouse_event(LEFTDOWN, 0, 0, 0, 0);
                    Thread.Sleep(1); // Small delay for click
                    mouse_event(LEFTUP, 0, 0, 0, 0);
                    Thread.Sleep(intervals); // Interval between clicks
                }
                else
                {
                    Thread.Sleep(100); // Longer delay when not clicking to reduce CPU usage
                }
            }
        }

        private void BackgroundWorker1_DoWork(object sender, DoWorkEventArgs e)
        {
            while (true) // Ensure this doesn't cause excessive CPU load
            {
                if (bap.Checked)
                {
                    if (GetAsyncKeyState(Keys.D) < 0)
                    {
                        click = false; // Stop clicking when 'D' is pressed
                    }

                    if (GetAsyncKeyState(Keys.A) < 0)
                    {
                        click = true; // Start clicking when 'A' is pressed
                    }
                }
                Thread.Sleep(100); // Interval for checking key states
            }
        }

        private void textBox2_TextChanged(object sender, EventArgs e)
        {
            // Validate if input is a valid integer
            if (!int.TryParse(textBox2.Text, out parsedValue))
            {
                MessageBox.Show("Please enter a valid number for the interval.");
                return; // Exit if not a valid number
            }

            intervals = parsedValue; // Update the intervals if valid
        }

        private void button18_Click(object sender, EventArgs e)
        {
            if (!int.TryParse(textBox2.Text, out parsedValue))
            {
                MessageBox.Show("Please enter a valid number, you idiot.");
                return;
            }

            intervals = parsedValue; // Update intervals from text box
        }

        private void backgroundWorker2_DoWork(object sender, DoWorkEventArgs e)
        {
            while (true) // Ensure this doesn't cause excessive CPU load
            {
                if (bap.Checked)
                {
                    if (GetAsyncKeyState(Keys.D) < 0)
                    {
                        click = false; // Stop clicking when 'D' is pressed
                    }

                    if (GetAsyncKeyState(Keys.A) < 0)
                    {
                        click = true; // Start clicking when 'A' is pressed
                    }
                }
                Thread.Sleep(100); // Interval for checking key states
            }
        }
    }
    }
}


