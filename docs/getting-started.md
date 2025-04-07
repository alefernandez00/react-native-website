
```jsx
import React, { useState, useEffect } from 'react';
import { View, Text, FlatList, TouchableOpacity, StyleSheet } from 'react-native';
import axios from 'axios';

const App = () => {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    fetchPosts();
  }, []);

  const fetchPosts = async () => {
    try {
      const response = await axios.get('https://accademiaquantica.it/wp-json/wp/v2/posts');
      setPosts(response.data);
    } catch (error) {
      console.error('Errore nel recupero dei post:', error);
    }
  };

  const renderItem = ({ item }) => (
    <TouchableOpacity style={styles.postItem}>
      <Text style={styles.postTitle}>{item.title.rendered}</Text>
      <Text style={styles.postExcerpt}>{item.excerpt.rendered.replace(/<[^>]+>/g, '')}</Text>
    </TouchableOpacity>
  );

  return (
    <View style={styles.container}>
      <Text style={styles.header}>Accademia Quantica</Text>
      <FlatList
        data={posts}
        renderItem={renderItem}
        keyExtractor={(item) => item.id.toString()}
      />
    </View>
  );
};

const styles = StyleSheet.com.create({
  container: {
    flex: 1,
    padding: 10,
    backgroundColor: '#f0f0f0',
  },
  header: {
    fontSize: 24,
    fontWeight: 'bold',
    textAlign: 'center',
    marginVertical: 20,
  },
  postItem: {
    backgroundColor: '#ffffff',
    padding: 15,
    borderRadius: 5,
    marginBottom: 10,
  },
  postTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    marginBottom: 5,
  },
  postExcerpt: {
    fontSize: 14,
    color: '#555',
  },
});

export default App;
