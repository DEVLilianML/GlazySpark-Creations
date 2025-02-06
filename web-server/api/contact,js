import mongoose from 'mongoose';

// Define the schema for the "glazy" collection
const contactSchema = new mongoose.Schema({
  name: String,
  phone: String,
  email: String,
  projectNarration: String,
  createdAt: { type: Date, default: Date.now },
});

// Create a model for the "glazy" collection
const Contact = mongoose.models.Contact || mongoose.model('Contact', contactSchema);

// Connect to MongoDB using the connection string stored in environment variables
const connectToDatabase = async () => {
  if (mongoose.connections[0].readyState) {
    // Already connected to MongoDB
    return;
  }

  await mongoose.connect(process.env.MONGODB_URI);
};

export default async function handler(req, res) {
  if (req.method === 'POST') {
    try {
      // Connect to MongoDB
      await connectToDatabase();

      // Get form data from the request body
      const { name, phone, email, projectNarration } = req.body;

      // Create a new document and save to the "glazy" collection
      const newContact = new Contact({
        name,
        phone,
        email,
        projectNarration,
      });

      await newContact.save(); // Save the document to MongoDB

      // Respond with a success message
      res.status(200).json({ message: 'Form submitted successfully!' });
    } catch (error) {
      console.error('Error saving form data:', error);
      res.status(500).json({ message: 'Error submitting form', error });
    }
  } else {
    res.status(405).json({ message: 'Method Not Allowed' });
  }
}

