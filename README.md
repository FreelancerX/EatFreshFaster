//# EatFreshFaster

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

/**
   The OrderCalculatorGUI class creates the GUI for the
   SubwayOrderTaker application.
*/

public class OrderCalculatorGUI extends JFrame
{
   private BagelPanel bagels;     // Bagel panel
   private ToppingPanel toppings; // Topping panel
   private CoffeePanel coffee;    // Coffee panel
   private GreetingPanel banner;  // To display a greeting

   private JPanel startPanel;		// Start panel
   private JTabbedPane mainPanel;	// Main panel
   private JPanel buttonPanel;    // To hold the buttons
   private JPanel checkoutPanel;    // To hold the buttons
   private JButton calcButton;    // To calculate the cost
   private JButton exitButton;    // To exit the application
   private JButton previousButton;// To go to previous tab
   private JButton nextButton;	  // To go to next tab
   private JButton startButton;

// Constants for set up of the window
   public static final int WIDTH = 600;
   public static final int HEIGHT = 600;
   private static final int TABS = 4; // number of tabs in main panel
   private final double TAX_RATE = 0.06; // Sales tax rate
   /**
      Constructor
   */

   public OrderCalculatorGUI()
   {
      // Display a title.
      setTitle("Order Calculator");

      //Sets default size of window
      setSize(WIDTH, HEIGHT);
      
      // Specify an action for the close button.
      setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);	// program keeps running until last orderCalc window is closed

      // Create a BorderLayout manager.
      setLayout(new BorderLayout());

      // Create the custom panels.
      banner = new GreetingPanel();
      bagels = new BagelPanel();
      toppings = new ToppingPanel();
      coffee = new CoffeePanel();

      // Create the start, button, main, and checkout panels.
      buildStartPanel();
      buildButtonPanel();
      buildMainPanel();
      buildCheckoutPanel();

      // Add the components to the content pane.
      add(banner, BorderLayout.NORTH);
      add(startPanel, BorderLayout.CENTER);

      // Display the contents of the window . 
      setVisible(true);
   }

   /**
      The buildStartPanel method builds the starting panel.
   */
   private void buildStartPanel()
   {
	   startPanel = new JPanel();			
	   startButton = new JButton("Start Order");
	   startButton.addActionListener(new StartButtonListener());
	   startPanel.add(startButton);
   }
   /**
   The buildMainPanel method builds the tabbed pane.
    */
   private void buildMainPanel()
   {
	  mainPanel = new JTabbedPane(JTabbedPane.RIGHT);	//creates TabbedPane, 
   	   
 	  JComponent panel1 = bagels;
 	  mainPanel.addTab("Tab 1", null, panel1);	//creates first tab using bagels panel
 	  
 	  
 	  JComponent panel2 = toppings;
 	  mainPanel.addTab("Tab 2", null, panel2); 
 	
 	  
 	  JComponent panel3 = coffee;
 	  mainPanel.addTab("Tab 3", null, panel3);
 	  
 	 JComponent panel4 = checkoutPanel;
	  mainPanel.addTab("Tab 4", null, panel4);
   }
   /**
   The buildButtonPanel method builds the button panel.
    */
   private void buildButtonPanel()
   {
      // Create a panel for the buttons.
      buttonPanel = new JPanel();

      // Create the buttons.      
      previousButton = new JButton("Previous");
      nextButton = new JButton("Next");
            
      calcButton = new JButton("Place Order");
      exitButton = new JButton("Cancel Order");

      // Register the action listeners.
      previousButton.addActionListener(new PreviousButtonListener());
      nextButton.addActionListener(new NextButtonListener());      
      calcButton.addActionListener(new CalcButtonListener());
      exitButton.addActionListener(new ExitButtonListener());

      // Add the buttons to the button panel.
      buttonPanel.add(previousButton);
      buttonPanel.add(nextButton);
      buttonPanel.add(calcButton);
      buttonPanel.add(exitButton);
      previousButton.setEnabled(false);
      calcButton.setEnabled(false);
   }
   /**
   The buildCheckoutPanel method builds the checkout panel.
    */
   private void buildCheckoutPanel()	//work in progress
   {
      // Create a panel for the buttons.
      checkoutPanel = new JPanel();

   // Variables to hold the subtotal, tax, and total
      double subtotal, tax, total;

      // Calculate the subtotal.
      subtotal = bagels.getBagelCost() + 
                 toppings.getToppingCost() +
                 coffee.getCoffeeCost();

      // Calculate the sales tax.
      tax = subtotal * TAX_RATE;

      // Calculate the total.
      total = subtotal + tax;

      // Display the charges.
       //(null, String.format("Subtotal: $%,.2f\n" +
       //                "Tax: $%,.2f\n" +
       //                "Total: $%,.2f",
       //                subtotal, tax, total));
   }
   /**
      Private inner class that handles the event when
      the user clicks the Calculate button.
   */

   private class CalcButtonListener implements ActionListener
   {
      public void actionPerformed(ActionEvent e)
      {
    	  mainPanel.setSelectedIndex(TABS-1);
      }
   }

   /**
      Private inner class that handles the event when
      the user clicks the Exit button.
   */

   private class ExitButtonListener implements ActionListener
   {
      public void actionPerformed(ActionEvent e)
      {
    	  new OrderCalculatorGUI();		//opens new order calculator window
    	  dispose();					//closes old window
      }
   }
   
   /**
   Private inner class that handles the event when
   the user clicks the Previous button.
    */
   private class PreviousButtonListener implements ActionListener
   {
      public void actionPerformed(ActionEvent e)
      {
    	  if(mainPanel.getSelectedIndex() > 0 || nextButton.isEnabled() == false)		
    	  {
    		  mainPanel.setSelectedIndex( mainPanel.getSelectedIndex()-1);	//enables the "next" button when any tab other than tab 1 is selected
    		  nextButton.setEnabled(true);
    	  }
    	  if(mainPanel.getSelectedIndex() <= 0)		//disables the "previous" button when tab 1 is selected
    		  previousButton.setEnabled(false);
      }
   }
   /**
   Private inner class that handles the event when
   the user clicks the Next button.
    */
   private class NextButtonListener implements ActionListener
   {
      public void actionPerformed(ActionEvent e)
      {
    	  if(mainPanel.getSelectedIndex() < TABS - 1 || previousButton.isEnabled() == false)		
    	  {
    		  mainPanel.setSelectedIndex( mainPanel.getSelectedIndex()+1);	//enables the "previous" button when any tab other than tab 1 is selected
    		  previousButton.setEnabled(true);
    	  }
    	  if(mainPanel.getSelectedIndex() >= TABS - 1)		//disables the "next" button when tab 1 is selected
    		  nextButton.setEnabled(false);
      }
   }
   /**
   Private inner class that handles the event when
   the user clicks the Start button.
    */
   private class StartButtonListener implements ActionListener
   {
      public void actionPerformed(ActionEvent e)
      {
    	  remove(startPanel);						//starts ordering process by adding the buttun panel to the bottom of the
    	  add(mainPanel, BorderLayout.CENTER);		//window, removing the start panel and replacing it with the main panel.
    	  add(buttonPanel, BorderLayout.SOUTH);		
    	  revalidate();								//refreshes the frame to show changes
      }
   }
   /**
      main method
   */
   
   public static void main(String[] args)
   {
      new OrderCalculatorGUI();
   }
}





